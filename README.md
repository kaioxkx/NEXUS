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
	tp = Window:AddTab({ Title = "ESPIONAGEM", Icon = "rbxassetid://126610247433890" }),
	
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

-- =====================================
-- ESP
-- =====================================

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local LocalPlayer = Players.LocalPlayer

local ESPEnabled = false
local ESPObjects = {}

-- cria esp
local function CreateESP(player)
    if player == LocalPlayer then return end
    if not player.Character then return end

    if ESPObjects[player] then
        ESPObjects[player]:Destroy()
        ESPObjects[player] = nil
    end

    local highlight = Instance.new("Highlight")
    highlight.Name = "NEXUS_ESP"
    highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
    highlight.FillTransparency = 1
    highlight.OutlineTransparency = 0.25
    highlight.Parent = player.Character

    ESPObjects[player] = highlight
end

-- remove esp
local function RemoveESP(player)
    if ESPObjects[player] then
        ESPObjects[player]:Destroy()
        ESPObjects[player] = nil
    end
end

-- atualiza cor do time
local function UpdateColor(player)
    local esp = ESPObjects[player]
    if not esp then return end

    if player.Team and player.Team.TeamColor then
        esp.OutlineColor = player.Team.TeamColor.Color
    else
        esp.OutlineColor = Color3.fromRGB(255,255,255)
    end
end

-- quando o personagem nascer
local function OnCharacterAdded(player, character)
    if ESPEnabled then
        task.wait(0.3)
        CreateESP(player)
        UpdateColor(player)
    end
end

-- quando player entrar
local function OnPlayerAdded(player)
    player.CharacterAdded:Connect(function(char)
        OnCharacterAdded(player, char)
    end)
end

-- conectar players atuais
for _, plr in ipairs(Players:GetPlayers()) do
    OnPlayerAdded(plr)
end

-- detectar novos players
Players.PlayerAdded:Connect(OnPlayerAdded)

-- loop de atualização (cor/time)
RunService.Heartbeat:Connect(function()
    if not ESPEnabled then return end

    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character then
            if not ESPObjects[player] then
                CreateESP(player)
            end
            UpdateColor(player)
        end
    end
end)

-- limpar tudo
local function ClearESP()
    for _, esp in pairs(ESPObjects) do
        if esp then
            esp:Destroy()
        end
    end
    ESPObjects = {}
end

-- TOGGLE
Tabs.Main:AddToggle("ESP_TOGGLE", {
    Title = "ESP",
    Default = false,
    Callback = function(Value)
        ESPEnabled = Value

        if not Value then
            ClearESP()
        else
            -- aplica geral quando liga
            for _, player in ipairs(Players:GetPlayers()) do
                if player ~= LocalPlayer and player.Character then
                    CreateESP(player)
                    UpdateColor(player)
                end
            end
        end
    end
})

-- =====================================
-- ESPIONAGEM 
-- =====================================


local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

local SelectedPlayer = nil
local SpectateEnabled = false
local PlayerButtons = {}

-- reset câmera
local function ResetCamera()
    if LocalPlayer.Character then
        local hum = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if hum then
            Camera.CameraSubject = hum
        end
    end
end

-- limpar botões
local function ClearButtons()
    for _, b in pairs(PlayerButtons) do
        b:Destroy()
    end
    PlayerButtons = {}
end

-- mostrar jogador selecionado (1 segundo)
local function ShowSelected(plr)
    local txt = Tabs.tp:AddParagraph({
        Title = "JOGADOR SELECIONADO",
        Content = plr.DisplayName .. " (@" .. plr.Name .. ")"
    })

    task.delay(1, function()
        txt:Destroy()
    end)
end

-- atualizar lista
local function RefreshPlayers()
    ClearButtons()

    for _, plr in ipairs(Players:GetPlayers()) do
        if plr ~= LocalPlayer then
            local btn = Tabs.tp:AddButton({
                Title = plr.DisplayName,
                Description = "(@" .. plr.Name .. ")",
                Callback = function()
                    SelectedPlayer = plr
                    ClearButtons()
                    ShowSelected(plr)
                end
            })

            table.insert(PlayerButtons, btn)
        end
    end
end

-- BOTÃO ESCOLHER
Tabs.tp:AddButton({
    Title = "ESCOLHER JOGADOR",
    Description = "Selecionar jogador do servidor",
    Callback = function()
        RefreshPlayers()
    end
})

-- BOTÃO TP (TP DENTRO DO PLAYER)
Tabs.tp:AddButton({
    Title = "TP",
    Description = "Teleportar para o jogador selecionado",
    Callback = function()
        if SelectedPlayer
        and SelectedPlayer.Character
        and SelectedPlayer.Character:FindFirstChild("HumanoidRootPart")
        and LocalPlayer.Character then

            LocalPlayer.Character:PivotTo(
                SelectedPlayer.Character.HumanoidRootPart.CFrame
            )
        end
    end
})

-- TOGGLE ESPECTAR
Tabs.tp:AddToggle("SPECTATE_TOGGLE", {
    Title = "ESPECTAR",
    Default = false,
    Callback = function(Value)
        SpectateEnabled = Value
        if not Value then
            ResetCamera()
        end
    end
})

-- se jogador sair
Players.PlayerRemoving:Connect(function(plr)
    if plr == SelectedPlayer then
        SelectedPlayer = nil
        SpectateEnabled = false
        ResetCamera()
    end
end)

-- espectar em tempo real
RunService.Heartbeat:Connect(function()
    if SpectateEnabled and SelectedPlayer then
        if SelectedPlayer.Character then
            local hum = SelectedPlayer.Character:FindFirstChildOfClass("Humanoid")
            if hum then
                Camera.CameraSubject = hum
            end
        end
    end
end)

-- =====================================
-- puxar informações 
-- =====================================


local Players = game:GetService("Players")
local HttpService = game:GetService("HttpService")

-- TÍTULO
Tabs.tp:AddParagraph({
    Title = "PUXADOR DE INFORMAÇÕES",
    Content = "Consulta de contas do Roblox"
})

-- função pra remover textos depois
local function AutoDestroy(obj, time)
    task.delay(time, function()
        if obj and obj.Destroy then
            obj:Destroy()
        end
    end)
end

-- INPUT USUÁRIO
Tabs.tp:AddInput("USER_LOOKUP", {
    Title = "USUÁRIO:",
    Default = "",
    Placeholder = "Digite o nome do usuário",
    Numeric = false,
    Finished = true,

    Callback = function(username)
        if username == "" then return end

        local userId

        -- tentar pegar ID pelo nome
        local success = pcall(function()
            userId = Players:GetUserIdFromNameAsync(username)
        end)

        -- se não existir
        if not success or not userId then
            Fluent:Notify({
                Title = "ERRO",
                Content = "USUÁRIO NÃO ENCONTRADO",
                Duration = 3
            })
            return
        end

        -- puxar infos da conta
        local info
        pcall(function()
            info = Players:GetUserInfosByUserIdsAsync({userId})[1]
        end)

        if not info then return end

        -- status
        local status = "Offline"
        if info.IsOnline then
            status = "Online"
        end

        -- data da conta
        local created = os.date("%d/%m/%Y", info.Created.UnixTimestamp)

        -- textos
        local t1 = Tabs.tp:AddParagraph({
            Title = "USUÁRIO:",
            Content = info.Username
        })

        local t2 = Tabs.tp:AddParagraph({
            Title = "ID:",
            Content = tostring(userId)
        })

        local t3 = Tabs.tp:AddParagraph({
            Title = "DATA DE CONTA:",
            Content = created
        })

        local t4 = Tabs.tp:AddParagraph({
            Title = "IDADE DA CONTA:",
            Content = tostring(info.AccountAge) .. " dias"
        })

        local t5 = Tabs.tp:AddParagraph({
            Title = "STATUS:",
            Content = status
        })

        -- remover depois de 60 segundos
        AutoDestroy(t1, 60)
        AutoDestroy(t2, 60)
        AutoDestroy(t3, 60)
        AutoDestroy(t4, 60)
        AutoDestroy(t5, 60)
    end
})

Options.MyToggle:SetValue(false)

Window:SelectTab(Inf)


spawn(monitorPlayerPosition)

