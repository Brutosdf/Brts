[KyzenHub.lua.txt](https://github.com/user-attachments/files/32086652/KyzenHub.lua.txt)
```lua
--// KYZEN HUB
--// UI Base - Preto / Amarelo

local Kyzen = {}

--==================================================
-- CONFIGURAÇÕES
--==================================================

Kyzеn.Name = "Kyzen Hub"
Kyzеn.Version = "1.0"

local MainColor = Color3.fromRGB(255, 195, 0)
local Background = Color3.fromRGB(8, 8, 8)
local SidebarColor = Color3.fromRGB(12, 12, 12)
local CardColor = Color3.fromRGB(17, 17, 17)
local TextColor = Color3.fromRGB(235, 235, 235)
local SubText = Color3.fromRGB(130, 130, 130)

--==================================================
-- SERVIÇOS
--==================================================

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")

local Player = Players.LocalPlayer
local PlayerGui = Player:WaitForChild("PlayerGui")

--==================================================
-- GUI
--==================================================

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "KyzenHub"
ScreenGui.ResetOnSpawn = false
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.Parent = PlayerGui

--==================================================
-- MAIN
--==================================================

local Main = Instance.new("Frame")
Main.Name = "Main"
Main.Size = UDim2.new(0, 720, 0, 450)
Main.Position = UDim2.new(0.5, -360, 0.5, -225)
Main.BackgroundColor3 = Background
Main.BorderSizePixel = 0
Main.Parent = ScreenGui

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 12)
MainCorner.Parent = Main

local MainStroke = Instance.new("UIStroke")
MainStroke.Color = Color3.fromRGB(40, 40, 40)
MainStroke.Thickness = 1
MainStroke.Parent = Main

--==================================================
-- ARRASTAR JANELA
--==================================================

local dragging = false
local dragStart
local startPos

Main.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = Main.Position
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then

        local delta = input.Position - dragStart

        Main.Position = UDim2.new(
            startPos.X.Scale,
            startPos.X.Offset + delta.X,
            startPos.Y.Scale,
            startPos.Y.Offset + delta.Y
        )
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)

--==================================================
-- SIDEBAR
--==================================================

local Sidebar = Instance.new("Frame")
Sidebar.Size = UDim2.new(0, 175, 1, 0)
Sidebar.BackgroundColor3 = SidebarColor
Sidebar.BorderSizePixel = 0
Sidebar.Parent = Main

local SidebarCorner = Instance.new("UICorner")
SidebarCorner.CornerRadius = UDim.new(0, 12)
SidebarCorner.Parent = Sidebar

--==================================================
-- LOGO
--==================================================

local Logo = Instance.new("TextLabel")
Logo.Size = UDim2.new(1, -20, 0, 45)
Logo.Position = UDim2.new(0, 10, 0, 15)
Logo.BackgroundTransparency = 1
Logo.Text = "KYZEN HUB"
Logo.TextColor3 = MainColor
Logo.TextSize = 21
Logo.Font = Enum.Font.GothamBold
Logo.Parent = Sidebar

local Version = Instance.new("TextLabel")
Version.Size = UDim2.new(1, 0, 0, 18)
Version.Position = UDim2.new(0, 0, 0, 48)
Version.BackgroundTransparency = 1
Version.Text = "VERSION 1.0"
Version.TextColor3 = SubText
Version.TextSize = 9
Version.Font = Enum.Font.Gotham
Version.Parent = Sidebar

--==================================================
-- CONTAINER DAS ABAS
--==================================================

local Tabs = Instance.new("Frame")
Tabs.Size = UDim2.new(1, -20, 1, -100)
Tabs.Position = UDim2.new(0, 10, 0, 90)
Tabs.BackgroundTransparency = 1
Tabs.Parent = Sidebar

local TabsLayout = Instance.new("UIListLayout")
TabsLayout.Padding = UDim.new(0, 7)
TabsLayout.SortOrder = Enum.SortOrder.LayoutOrder
TabsLayout.Parent = Tabs

--==================================================
-- CONTENT
--==================================================

local Content = Instance.new("Frame")
Content.Size = UDim2.new(1, -195, 1, 0)
Content.Position = UDim2.new(0, 195, 0, 0)
Content.BackgroundTransparency = 1
Content.Parent = Main

--==================================================
-- TÍTULO
--==================================================

local PageTitle = Instance.new("TextLabel")
PageTitle.Size = UDim2.new(1, -40, 0, 45)
PageTitle.Position = UDim2.new(0, 20, 0, 15)
PageTitle.BackgroundTransparency = 1
PageTitle.Text = "Dashboard"
PageTitle.TextColor3 = TextColor
PageTitle.TextSize = 24
PageTitle.TextXAlignment = Enum.TextXAlignment.Left
PageTitle.Font = Enum.Font.GothamBold
PageTitle.Parent = Content

local Line = Instance.new("Frame")
Line.Size = UDim2.new(1, -40, 0, 1)
Line.Position = UDim2.new(0, 20, 0, 62)
Line.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
Line.BorderSizePixel = 0
Line.Parent = Content

--==================================================
-- PÁGINAS
--==================================================

local Pages = {}

local function CreatePage(name)

    local Page = Instance.new("ScrollingFrame")
    Page.Name = name
    Page.Size = UDim2.new(1, -40, 1, -85)
    Page.Position = UDim2.new(0, 20, 0, 75)
    Page.BackgroundTransparency = 1
    Page.BorderSizePixel = 0
    Page.ScrollBarThickness = 3
    Page.ScrollBarImageColor3 = MainColor
    Page.Visible = false
    Page.CanvasSize = UDim2.new(0, 0, 0, 0)
    Page.Parent = Content

    local Layout = Instance.new("UIListLayout")
    Layout.Padding = UDim.new(0, 10)
    Layout.SortOrder = Enum.SortOrder.LayoutOrder
    Layout.Parent = Page

    Layout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        Page.CanvasSize = UDim2.new(
            0,
            0,
            0,
            Layout.AbsoluteContentSize.Y + 15
        )
    end)

    Pages[name] = Page

    return Page
end

--==================================================
-- BOTÃO DE ABA
--==================================================

local CurrentPage

local function CreateTab(name, icon, page)

    local Button = Instance.new("TextButton")
    Button.Size = UDim2.new(1, 0, 0, 38)
    Button.BackgroundColor3 = SidebarColor
    Button.BorderSizePixel = 0
    Button.Text = icon .. "   " .. name
    Button.TextColor3 = SubText
    Button.TextSize = 12
    Button.TextXAlignment = Enum.TextXAlignment.Left
    Button.Font = Enum.Font.GothamMedium
    Button.AutoButtonColor = false
    Button.Parent = Tabs

    local Padding = Instance.new("UIPadding")
    Padding.PaddingLeft = UDim.new(0, 12)
    Padding.Parent = Button

    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 8)
    Corner.Parent = Button

    Button.MouseButton1Click:Connect(function()

        for _, p in pairs(Pages) do
            p.Visible = false
        end

        page.Visible = true
        CurrentPage = page

        for _, b in pairs(Tabs:GetChildren()) do
            if b:IsA("TextButton") then
                b.BackgroundColor3 = SidebarColor
                b.TextColor3 = SubText
            end
        end

        Button.BackgroundColor3 = MainColor
        Button.TextColor3 = Color3.fromRGB(0, 0, 0)

        PageTitle.Text = name
    end)

    return Button
end

--==================================================
-- CARD
--==================================================

local function CreateCard(page, title, description, buttonText, callback)

    local Card = Instance.new("Frame")
    Card.Size = UDim2.new(1, -5, 0, 82)
    Card.BackgroundColor3 = CardColor
    Card.BorderSizePixel = 0
    Card.Parent = page

    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 9)
    Corner.Parent = Card

    local Stroke = Instance.new("UIStroke")
    Stroke.Color = Color3.fromRGB(35, 35, 35)
    Stroke.Thickness = 1
    Stroke.Parent = Card

    local Title = Instance.new("TextLabel")
    Title.Size = UDim2.new(1, -150, 0, 25)
    Title.Position = UDim2.new(0, 15, 0, 12)
    Title.BackgroundTransparency = 1
    Title.Text = title
    Title.TextColor3 = TextColor
    Title.TextSize = 14
    Title.TextXAlignment = Enum.TextXAlignment.Left
    Title.Font = Enum.Font.GothamBold
    Title.Parent = Card

    local Description = Instance.new("TextLabel")
    Description.Size = UDim2.new(1, -160, 0, 30)
    Description.Position = UDim2.new(0, 15, 0, 38)
    Description.BackgroundTransparency = 1
    Description.Text = description
    Description.TextColor3 = SubText
    Description.TextSize = 10
    Description.TextXAlignment = Enum.TextXAlignment.Left
    Description.Font = Enum.Font.Gotham
    Description.Parent = Card

    local Button = Instance.new("TextButton")
    Button.Size = UDim2.new(0, 105, 0, 35)
    Button.Position = UDim2.new(1, -120, 0.5, -17)
    Button.BackgroundColor3 = MainColor
    Button.BorderSizePixel = 0
    Button.Text = buttonText
    Button.TextColor3 = Color3.fromRGB(0, 0, 0)
    Button.TextSize = 11
    Button.Font = Enum.Font.GothamBold
    Button.AutoButtonColor = false
    Button.Parent = Card

    local ButtonCorner = Instance.new("UICorner")
    ButtonCorner.CornerRadius = UDim.new(0, 7)
    ButtonCorner.Parent = Button

    Button.MouseButton1Click:Connect(function()

        if callback then
            callback()
        end

    end)

    return Card
end

--==================================================
-- PÁGINAS
--==================================================

local HomePage = CreatePage("Dashboard")
local ScriptsPage = CreatePage("Scripts")
local PremiumPage = CreatePage("Premium")
local SettingsPage = CreatePage("Settings")

--==================================================
-- ABAS
--==================================================

CreateTab("Dashboard", "⌂", HomePage)
CreateTab("Scripts", "◆", ScriptsPage)
CreateTab("Premium", "★", PremiumPage)
CreateTab("Settings", "⚙", SettingsPage)

--==================================================
-- DASHBOARD
--==================================================

CreateCard(
    HomePage,
    "Welcome to Kyzen Hub",
    "Painel principal do seu hub.",
    "Abrir",
    function()
        print("Kyzen Hub iniciado!")
    end
)

CreateCard(
    HomePage,
    "Scripts",
    "Acesse sua biblioteca de scripts.",
    "Scripts",
    function()

        for _, p in pairs(Pages) do
            p.Visible = false
        end

        ScriptsPage.Visible = true
        PageTitle.Text = "Scripts"

    end
)

CreateCard(
    HomePage,
    "Kyzen Premium",
    "Recursos exclusivos da versão Premium.",
    "Premium",
    function()

        for _, p in pairs(Pages) do
            p.Visible = false
        end

        PremiumPage.Visible = true
        PageTitle.Text = "Premium"

    end
)

--==================================================
-- SCRIPTS
--==================================================

CreateCard(
    ScriptsPage,
    "Script 01",
    "Coloque aqui a descrição do seu script.",
    "Executar",
    function()

        -- COLOQUE A FUNÇÃO DO SEU SCRIPT AQUI

        print("Script 01 selecionado")

    end
)

CreateCard(
    ScriptsPage,
    "Script 02",
    "Outro script do Kyzen Hub.",
    "Executar",
    function()

        -- COLOQUE A FUNÇÃO DO SEU SCRIPT AQUI

        print("Script 02 selecionado")

    end
)

CreateCard(
    ScriptsPage,
    "Script 03",
    "Adicione quantos scripts quiser.",
    "Executar",
    function()

        -- COLOQUE A FUNÇÃO DO SEU SCRIPT AQUI

        print("Script 03 selecionado")

    end
)

--==================================================
-- PREMIUM
--==================================================

CreateCard(
    PremiumPage,
    "Kyzen Premium",
    "Área exclusiva para usuários Premium.",
    "Abrir",
    function()

        print("Área Premium")

    end
)

CreateCard(
    PremiumPage,
    "Premium Script",
    "Adicione aqui seu recurso Premium.",
    "Executar",
    function()

        -- SUA FUNÇÃO PREMIUM AQUI

        print("Premium Script selecionado")

    end
)

--==================================================
-- SETTINGS
--==================================================

CreateCard(
    SettingsPage,
    "Recarregar UI",
    "Reinicia a interface do Kyzen Hub.",
    "Recarregar",
    function()

        ScreenGui.Enabled = false
        task.wait(0.2)
        ScreenGui.Enabled = true

    end
)

CreateCard(
    SettingsPage,
    "Fechar Hub",
    "Fecha a interface do Kyzen Hub.",
    "Fechar",
    function()

        ScreenGui:Destroy()

    end
)

--==================================================
-- ABRIR DASHBOARD
--==================================================

HomePage.Visible = true
CurrentPage = HomePage

for _, button in pairs(Tabs:GetChildren()) do

    if button:IsA("TextButton") then

        button.BackgroundColor3 = SidebarColor
        button.TextColor3 = SubText

    end

end

local FirstButton = Tabs:GetChildren()[1]

if FirstButton and FirstButton:IsA("TextButton") then
    FirstButton.BackgroundColor3 = MainColor
    FirstButton.TextColor3 = Color3.fromRGB(0, 0, 0)
end

print("Kyzen Hub carregado!")
```
