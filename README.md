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

local UIS = game:GetService("UserInputService")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local LocalPlayer = Players.LocalPlayer
local InfiniteJumpEnabled = false
local NoClipEnabled = false

-- =====================================
-- INFINITEJUMP
-- =====================================
Tabs.Main:AddToggle("INFINITEJUMP_TOGGLE", {
    Title = "INFINITEJUMP",
    Default = false,
    Callback = function(Value)
        InfiniteJumpEnabled = Value
    end
})

-- executa o InfiniteJump
UIS.JumpRequest:Connect(function()
    if InfiniteJumpEnabled then
        local character = LocalPlayer.Character
        if character then
            local humanoid = character:FindFirstChildOfClass("Humanoid")
            if humanoid and humanoid:GetState() ~= Enum.HumanoidStateType.Jumping then
                humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
            end
        end
    end
end)

-- =====================================
-- NOCLIP
-- =====================================
Tabs.Main:AddToggle("NOCLIP_TOGGLE", {
    Title = "NOCLIP",
    Default = false,
    Callback = function(Value)
        NoClipEnabled = Value
    end
})

-- loop do noclip
RunService.Stepped:Connect(function()
    if NoClipEnabled then
        local character = LocalPlayer.Character
        if character then
            for _, part in ipairs(character:GetDescendants()) do
                if part:IsA("BasePart") and part.CanCollide then
                    part.CanCollide = false
                end
            end
        end
    else
        local character = LocalPlayer.Character
        if character then
            for _, part in ipairs(character:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.CanCollide = true
                end
            end
        end
    end
end)
-- =====================================
-- esp
-- =====================================

-- SERVIÇOS
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local LocalPlayer = Players.LocalPlayer
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
    highlight.DepthMode = Enum.HighlightDepthMode.Occluded
    highlight.FillTransparency = 1
    highlight.OutlineTransparency = 0

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
-- FUNÇÃO: APLICAR ESP EM TODOS OS JOGADORES
-- =====================================
local function applyESPAll()
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer then
            applyESP(player)
        end
    end
end

-- =====================================
-- FUNÇÃO: REMOVER ESP DE TODOS
-- =====================================
local function removeESPAll()
    for player, _ in pairs(ESPObjects) do
        removeESP(player)
    end
end

-- =====================================
-- RESPAWN: GARANTE ESP
-- =====================================
local function onCharacterAdded(player)
    player.CharacterAdded:Connect(function()
        task.wait(0.3) -- espera o character carregar
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
-- LOOP PRINCIPAL: ATUALIZA ESP TODO MOMENTO
-- =====================================
RunService.Heartbeat:Connect(function()
    if ESPEnabled then
        for player, highlight in pairs(ESPObjects) do
            if player.Team and player.Team.TeamColor then
                highlight.OutlineColor = player.Team.TeamColor.Color
            else
                highlight.OutlineColor = Color3.new(1,1,1)
            end
        end

        -- garante ESP em todo mundo
        applyESPAll()
    end
end)

-- =====================================
-- BOTÃO ESP ON/OFF
-- =====================================
Tabs.Main:AddToggle("ESP_TOGGLE", {
    Title = "ESP",
    Default = false,
    Callback = function(Value)
        ESPEnabled = Value

        if not ESPEnabled then
            removeESPAll() -- remove tudo instantaneamente
        else
            applyESPAll() -- aplica todos ao ligar
        end
    end
})
Options.MyToggle:SetValue(false)

Window:SelectTab(Inf)


spawn(monitorPlayerPosition)

