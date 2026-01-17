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
            Title = "MILENIO X",
            Content = "RESET",
            SubContent = "Velocidade, pulo e gravidade resetados",
            Duration = 4
        })
    end
})

-- esp
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local LocalPlayer = Players.LocalPlayer
local ESPEnabled = false
local ESPObjects = {}

-- CRIAR ESP LEVE
local function createESP(player)
    if player == LocalPlayer then return end
    if not player.Character then return end
    if ESPObjects[player] then return end

    local highlight = Instance.new("Highlight")
    highlight.Name = "MilenioX_ESP"
    highlight.Adornee = player.Character
    highlight.DepthMode = Enum.HighlightDepthMode.Occluded -- clássico
    highlight.FillTransparency = 1
    highlight.OutlineTransparency = 0

    -- cor do time
    if player.Team and player.Team.TeamColor then
        highlight.OutlineColor = player.Team.TeamColor.Color
    else
        highlight.OutlineColor = Color3.new(1,1,1)
    end

    highlight.Parent = player.Character
    ESPObjects[player] = highlight
end

-- REMOVER ESP
local function removeESP(player)
    if ESPObjects[player] then
        ESPObjects[player]:Destroy()
        ESPObjects[player] = nil
    end
end

-- ATUALIZA TODO MOMENTO
RunService.Heartbeat:Connect(function()
    if not ESPEnabled then return end

    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character then
            createESP(player) -- cria se não existir

            -- atualiza cor do time
            if player.Team and player.Team.TeamColor then
                ESPObjects[player].OutlineColor = player.Team.TeamColor.Color
            else
                ESPObjects[player].OutlineColor = Color3.new(1,1,1)
            end
        end
    end
end)

-- RESPAWN / ENTRADA DE JOGADOR
Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function()
        task.wait(0.3) -- espera o personagem carregar
        if ESPEnabled then
            createESP(player)
        end
    end)
end)

for _, player in ipairs(Players:GetPlayers()) do
    player.CharacterAdded:Connect(function()
        task.wait(0.3)
        if ESPEnabled then
            createESP(player)
        end
    end)
end

-- BOTÃO ESP CLÁSSICO
Tabs.Main:AddToggle("ESP_TOGGLE", {
    Title = "ESP (Team Color)",
    Default = false,
    Callback = function(Value)
        ESPEnabled = Value

        if not ESPEnabled then
            for player, _ in pairs(ESPObjects) do
                removeESP(player)
            end
        else
            -- ativa ESP em todos que já estão no jogo
            for _, player in ipairs(Players:GetPlayers()) do
                if player ~= LocalPlayer then
                    createESP(player)
                end
            end
        end
    end
})
Options.MyToggle:SetValue(false)

Window:SelectTab(Inf)


spawn(monitorPlayerPosition)

