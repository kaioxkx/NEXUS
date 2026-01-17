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
-- NEXUS - INFINITEJUMP & NOCLIP
-- =====================================

-- SERVIÇOS
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

local LocalPlayer = Players.LocalPlayer

-- VARIÁVEIS
local InfiniteJumpEnabled = false
local NoclipEnabled = false
local NoclipParts = {}

-- =====================================
-- INFINITEJUMP
-- =====================================
UserInputService.JumpRequest:Connect(function()
    if InfiniteJumpEnabled then
        local character = LocalPlayer.Character
        if character then
            local humanoid = character:FindFirstChildOfClass("Humanoid")
            if humanoid and humanoid.Health > 0 then
                humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
            end
        end
    end
end)

Tabs.Main:AddToggle("INFINITEJUMP", {
    Title = "INFINITEJUMP",
    Default = false,
    Callback = function(Value)
        InfiniteJumpEnabled = Value
    end
})

-- =====================================
-- NOCLIP
-- =====================================
local function setNoclip(enable)
    local character = LocalPlayer.Character
    if not character then return end

    if enable then
        -- salva valores originais e desativa colisão
        NoclipParts = {}
        for _, part in ipairs(character:GetDescendants()) do
            if part:IsA("BasePart") then
                NoclipParts[part] = part.CanCollide
                part.CanCollide = false
            end
        end
    else
        -- restaura colisões
        for part, original in pairs(NoclipParts) do
            if part and part.Parent then
                part.CanCollide = original
            end
        end
        NoclipParts = {}
    end
end

-- aplica noclip constantemente se ligado
RunService.Stepped:Connect(function()
    if NoclipEnabled then
        setNoclip(true)
    end
end)

Tabs.Main:AddToggle("NOCLIP", {
    Title = "NOCLIP",
    Default = false,
    Callback = function(Value)
        NoclipEnabled = Value
        setNoclip(NoclipEnabled)
    end
})

-- ===============================
-- ESP CLÁSSICO PREMIUM 
-- ===============================

-- SERVIÇOS
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local LocalPlayer = Players.LocalPlayer
local ESPEnabled = false
local ESPObjects = {}

-- FUNÇÃO PRA CRIAR ESP HIGHLIGHT
local function createPlayerESP(player)
    if player == LocalPlayer then
        return
    end

    if ESPObjects[player] then
        return
    end

    -- precisa ter character
    if not player.Character or not player.Character.Parent then
        return
    end

    local highlight = Instance.new("Highlight")
    highlight.Name = "MilenioX_ESP"
    highlight.Adornee = player.Character
    highlight.FillTransparency = 1
    highlight.OutlineTransparency = 0
    highlight.DepthMode = Enum.HighlightDepthMode.Occluded

    if player.Team and player.Team.TeamColor then
        highlight.OutlineColor = player.Team.TeamColor.Color
    else
        highlight.OutlineColor = Color3.new(1,1,1)
    end

    highlight.Parent = player.Character
    ESPObjects[player] = highlight
end

-- FUNÇÃO PRA REMOVER ESP
local function removePlayerESP(player)
    if ESPObjects[player] then
        ESPObjects[player]:Destroy()
        ESPObjects[player] = nil
    end
end

-- APLICA ESP EM TODOS
local function applyAllESP()
    for _, plr in ipairs(Players:GetPlayers()) do
        if plr ~= LocalPlayer then
            createPlayerESP(plr)
        end
    end
end

-- REMOVE ESP DE TODOS
local function removeAllESP()
    for plr, _ in pairs(ESPObjects) do
        removePlayerESP(plr)
    end
end

-- GARANTE ESP QUANDO UM PLAYER RESPAWNA
local function onCharacterAddedToPlayer(player)
    player.CharacterAdded:Connect(function()
        task.wait(0.3)
        if ESPEnabled then
            createPlayerESP(player)
        end
    end)
end

-- CONFIGURA ESP PRA EXISTENTES
for _, plr in ipairs(Players:GetPlayers()) do
    onCharacterAddedToPlayer(plr)
end

-- CONFIGURA ESP PRA QUEM ENTROU DEPOIS
Players.PlayerAdded:Connect(function(plr)
    onCharacterAddedToPlayer(plr)
end)

-- LOOP QUE ATUALIZA A COR DO TIME
RunService.Heartbeat:Connect(function()
    if not ESPEnabled then
        return
    end

    for plr, high in pairs(ESPObjects) do
        if plr.Team and plr.Team.TeamColor then
            high.OutlineColor = plr.Team.TeamColor.Color
        else
            high.OutlineColor = Color3.new(1,1,1)
        end
    end

    applyAllESP() -- garante que todo mundo tem highlight
end)

-- BOTÃO ESP ON/OFF
Tabs.Main:AddToggle("ESP_TOGGLE", {
    Title = "ESP",
    Default = false,
    Callback = function(Value)
        ESPEnabled = Value

        if ESPEnabled then
            applyAllESP()
        else
            removeAllESP()
        end
    end
})
Options.MyToggle:SetValue(false)

Window:SelectTab(Inf)


spawn(monitorPlayerPosition)

