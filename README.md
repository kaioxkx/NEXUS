local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()

local Window = Fluent:CreateWindow({
    Title = "NEXUS",
    SubTitle = "POR KAIOX",
    TabWidth = 160,
    Size = UDim2.fromOffset(470, 260),
    Acrylic = false,
    Theme = "Darker",
    MinimizeKey = Enum.KeyCode.LeftAlt
})

local Tabs = {
    Inf = Window:AddTab({ Title = "INÍCIO", Icon = "rbxassetid://98840793003231" }),
    Main = Window:AddTab({ Title = "DIVERSÃO", Icon = "rbxassetid://130912825937739" }),
	troll = Window:AddTab({ Title = "TROOL", Icon = "rbxassetid://126610247433890" }),
	
}

local Players = game:GetService("Players")
local MarketplaceService = game:GetService("MarketplaceService")

local player = Players.LocalPlayer

-- Nome do jogo
local gameName = "Desconhecido"
pcall(function()
    gameName = MarketplaceService:GetProductInfo(game.PlaceId).Name
end)

-- Executor
local executor = "Desconhecido"
if identifyexecutor then
    executor = identifyexecutor()
end

Tabs.Inf:AddButton({
    Title = "DISCORD",
    Description = "COPIAR LINK DO DISCORD",
    Callback = function()
        setclipboard("https://discord.gg/HwmQsvXyv6")

        Fluent:Notify({
            Title = "NEXUS",
            Content = "DISCORD",
            SubContent = "LINK DO DISCORD COPIADO✅",
            Duration = 3
        })
    end
})

-- INFO DO JOGADOR
Tabs.Inf:AddParagraph({
    Title = "NOME:",
    Content = player.Name
})

Tabs.Inf:AddParagraph({
    Title = "ID:",
    Content = tostring(player.UserId)
})

Tabs.Inf:AddParagraph({
    Title = "JOGO ATUAL:",
    Content = gameName
})

Tabs.Inf:AddParagraph({
    Title = "EXECUTOR:",
    Content = executor
})
-- SERVIÇOS
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer

-- VALORES PADRÃO (CERTOS)
local SpeedValue = 16
local JumpValue = 50
local GravityValue = 196.2

-- INPUT VELOCIDADE
local SpeedInput = Tabs.Main:AddInput("SpeedInput", {
    Title = "Velocidade",
    Default = tostring(SpeedValue),
    Placeholder = "Ex: 16",
    Numeric = true,
    Finished = false,
    Callback = function(Value)
        if tonumber(Value) then
            SpeedValue = tonumber(Value)
        end
    end
})

-- INPUT PULO
local JumpInput = Tabs.Main:AddInput("JumpInput", {
    Title = "Pulo",
    Default = tostring(JumpValue),
    Placeholder = "Ex: 50",
    Numeric = true,
    Finished = false,
    Callback = function(Value)
        if tonumber(Value) then
            JumpValue = tonumber(Value)
        end
    end
})

-- INPUT GRAVIDADE
local GravityInput = Tabs.Main:AddInput("GravityInput", {
    Title = "Gravidade",
    Default = tostring(GravityValue),
    Placeholder = "Ex: 196.2",
    Numeric = true,
    Finished = false,
    Callback = function(Value)
        if tonumber(Value) then
            GravityValue = tonumber(Value)
        end
    end
})

-- APLICA TUDO O TEMPO TODO
RunService.Stepped:Connect(function()
    local character = player.Character
    if character then
        local humanoid = character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid.WalkSpeed = SpeedValue
            humanoid.JumpPower = JumpValue
        end
    end

    workspace.Gravity = GravityValue
	
end)
Tabs.Main:AddButton({
    Title = "RESET STATUS",
    Description = "Reseta velocidade, pulo e gravidade",
    Callback = function()
        -- valores padrão do Roblox
        SpeedValue = 16
        JumpValue = 50
        GravityValue = 196.2

        -- atualiza os inputs
        SpeedInput:SetValue(tostring(SpeedValue))
        JumpInput:SetValue(tostring(JumpValue))
        GravityInput:SetValue(tostring(GravityValue))

        Fluent:Notify({
            Title = "NEXUS",
            Content = "RESET",
            SubContent = "VELOCIDADE,PULO E GRAVIDADE FORAM RESETADOS!",
            Duration = 3
        })
    end
})

-- =====================================
-- ESP
-- =====================================

-- SERVIÇOS
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- VARIÁVEIS
local ESPEnabled = false
local ESPObjects = {}

-- =====================================
-- FUNÇÃO: CRIAR ESP
-- =====================================
local function applyESP(player)
    if player == LocalPlayer then return end
    if ESPObjects[player] then return end

    local character = player.Character
    if not character then return end

    local highlight = Instance.new("Highlight")
    highlight.Name = "MilenioX_ESP"
    highlight.Adornee = character
    highlight.DepthMode = Enum.HighlightDepthMode.Occluded  -- clássico
    highlight.FillTransparency = 1
    highlight.OutlineTransparency = 0

    -- cor do time
    if player.Team and player.Team.TeamColor then
        highlight.OutlineColor = player.Team.TeamColor.Color
    else
        highlight.OutlineColor = Color3.new(1,1,1)
    end

    highlight.Parent = character
    ESPObjects[player] = highlight
end

-- =====================================
-- FUNÇÃO: REMOVER ESP
-- =====================================
local function removeESP(player)
    if ESPObjects[player] then
        ESPObjects[player]:Destroy()
        ESPObjects[player] = nil
    end
end

-- =====================================
-- ATUALIZA ESP QUANDO MUDAR DE TIME
-- =====================================
Players.PlayerAdded:Connect(function(player)
    player:GetPropertyChangedSignal("Team"):Connect(function()
        if ESPEnabled and ESPObjects[player] then
            if player.Team and player.Team.TeamColor then
                ESPObjects[player].OutlineColor = player.Team.TeamColor.Color
            else
                ESPObjects[player].OutlineColor = Color3.new(1,1,1)
            end
        end
    end)
end)

-- =====================================
-- RESPAWN: GARANTE ESP
-- =====================================
local function onCharacterAdded(player)
    player.CharacterAdded:Connect(function()
        task.wait(0.3)  -- espera o character carregar
        if ESPEnabled then
            applyESP(player)
        end
    end)
end

-- conecta para todos já no jogo
for _, player in ipairs(Players:GetPlayers()) do
    onCharacterAdded(player)
end

-- conecta jogadores que entrarem depois
Players.PlayerAdded:Connect(function(player)
    onCharacterAdded(player)
end)

-- =====================================
-- BOTÃO ESP ON/OFF
-- =====================================
Tabs.Main:AddToggle("ESP_TOGGLE", {
    Title = "ESP Clássico (Team Color)",
    Default = false,
    Callback = function(Value)
        ESPEnabled = Value

        if ESPEnabled then
            -- aplica ESP para todos já presentes
            for _, player in ipairs(Players:GetPlayers()) do
                if player ~= LocalPlayer then
                    applyESP(player)
                end
            end
        else
            -- remove todos
            for player, _ in pairs(ESPObjects) do
                removeESP(player)
            end
        end
    end
})
Options.MyToggle:SetValue(false)

Window:SelectTab(Inf)


spawn(monitorPlayerPosition)

