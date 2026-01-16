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
    Inf = Window:AddTab({ Title = "COMEÇO", Icon = "rbxassetid://79247048304044" }),
    Main = Window:AddTab({ Title = "DIVERSÃO", Icon = "rbxassetid://106204366458572" }),
	troll = Window:AddTab({ Title = "TROOL", Icon = "rbxassetid://106204366458572" }),
}

local Players = game:GetService("Players")
local MarketplaceService = game:GetService("MarketplaceService")

local player = Players.LocalPlayer

-- Nome do jogo
local gameName = "Desconhecido"
pcall(function()
    gameName = MarketplaceService:GetProductInfo(game.PlaceId).Name
end)

-- Executor (tipo de Roblox)
local executor = "Desconhecido"
if identifyexecutor then
    executor = identifyexecutor()
end

-- TÍTULO
Tabs.Inf:AddParagraph({
    Title = "BEM VINDO!",
    Content = ""
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
Options.MyToggle:SetValue(false)

Window:SelectTab(1)


spawn(monitorPlayerPosition)

