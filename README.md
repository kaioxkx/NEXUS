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
            Duration = 5
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
Options.MyToggle:SetValue(false)

Window:SelectTab(Tabs.Inf)


spawn(monitorPlayerPosition)

