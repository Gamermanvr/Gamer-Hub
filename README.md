# Gamer-Hub
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Color Palette (Black & Gold)
local COLORS = {
	Background = Color3.fromRGB(16, 16, 18),
	Sidebar = Color3.fromRGB(22, 22, 25),
	Card = Color3.fromRGB(28, 28, 32),
	CardHover = Color3.fromRGB(38, 36, 30),
	GoldAccent = Color3.fromRGB(218, 175, 55),
	TextPrimary = Color3.fromRGB(245, 245, 245),
	TextSecondary = Color3.fromRGB(160, 160, 165)
}

-- Create Main ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "GamerHubUI"
screenGui.ResetOnSpawn = false
screenGui.Parent = playerGui

-- Main Window Frame
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainWindow"
mainFrame.Size = UDim2.new(0, 680, 0, 440)
mainFrame.Position = UDim2.new(0.5, -340, 0.5, -220)
mainFrame.BackgroundColor3 = COLORS.Background
mainFrame.BorderSizePixel = 0
mainFrame.Parent = screenGui

local mainCorner = Instance.new("UICorner")
mainCorner.CornerRadius = UDim.new(0, 10)
mainCorner.Parent = mainFrame

local mainStroke = Instance.new("UIStroke")
mainStroke.Color = COLORS.GoldAccent
mainStroke.Thickness = 1.5
mainStroke.Parent = mainFrame

-- Left Sidebar
local sidebar = Instance.new("Frame")
sidebar.Name = "Sidebar"
sidebar.Size = UDim2.new(0, 180, 1, 0)
sidebar.BackgroundColor3 = COLORS.Sidebar
sidebar.BorderSizePixel = 0
sidebar.Parent = mainFrame

local sidebarCorner = Instance.new("UICorner")
sidebarCorner.CornerRadius = UDim.new(0, 10)
sidebarCorner.Parent = sidebar

-- Header Title ("GH Gamer Hub")
local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, -20, 0, 50)
titleLabel.Position = UDim2.new(0, 15, 0, 5)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "GH  Gamer Hub"
titleLabel.TextColor3 = COLORS.GoldAccent
titleLabel.Font = Enum.Font.GothamBold
titleLabel.TextSize = 18
titleLabel.TextXAlignment = Enum.TextXAlignment.Left
titleLabel.Parent = sidebar

-- Profile Display at Bottom Left
local profileLabel = Instance.new("TextLabel")
profileLabel.Size = UDim2.new(1, -20, 0, 35)
profileLabel.Position = UDim2.new(0, 15, 1, -45)
profileLabel.BackgroundTransparency = 1
profileLabel.Text = "@" .. player.Name
profileLabel.TextColor3 = COLORS.TextSecondary
profileLabel.Font = Enum.Font.GothamMedium
profileLabel.TextSize = 13
profileLabel.TextXAlignment = Enum.TextXAlignment.Left
profileLabel.Parent = sidebar

-- Content Container (Right Side)
local contentContainer = Instance.new("Frame")
contentContainer.Name = "ContentContainer"
contentContainer.Size = UDim2.new(1, -195, 1, -20)
contentContainer.Position = UDim2.new(0, 190, 0, 10)
contentContainer.BackgroundTransparency = 1
contentContainer.Parent = mainFrame

-- Tab Pages Storage
local pages = {}
local tabButtons = {}

local function createTab(tabName, order)
	-- Sidebar Button
	local tabBtn = Instance.new("TextButton")
	tabBtn.Name = tabName .. "Tab"
	tabBtn.Size = UDim2.new(1, -20, 0, 38)
	tabBtn.Position = UDim2.new(0, 10, 0, 60 + ((order - 1) * 46))
	tabBtn.BackgroundColor3 = COLORS.Card
	tabBtn.Text = "  " .. tabName
	tabBtn.TextColor3 = COLORS.TextPrimary
	tabBtn.Font = Enum.Font.GothamSemibold
	tabBtn.TextSize = 14
	tabBtn.TextXAlignment = Enum.TextXAlignment.Left
	tabBtn.AutoButtonColor = false
	tabBtn.Parent = sidebar

	local btnCorner = Instance.new("UICorner")
	btnCorner.CornerRadius = UDim.new(0, 6)
	btnCorner.Parent = tabBtn

	local btnStroke = Instance.new("UIStroke")
	btnStroke.Color = COLORS.GoldAccent
	btnStroke.Thickness = 1
	btnStroke.Enabled = (order == 1)
	btnStroke.Parent = tabBtn

	-- Page ScrollingFrame
	local page = Instance.new("ScrollingFrame")
	page.Name = tabName .. "Page"
	page.Size = UDim2.new(1, 0, 1, 0)
	page.BackgroundTransparency = 1
	page.ScrollBarThickness = 4
	page.ScrollBarImageColor3 = COLORS.GoldAccent
	page.Visible = (order == 1)
	page.Parent = contentContainer

	local gridLayout = Instance.new("UIGridLayout")
	gridLayout.CellSize = UDim2.new(0.48, 0, 0, 72)
	gridLayout.CellPadding = UDim2.new(0.03, 0, 0, 10)
	gridLayout.SortOrder = Enum.SortOrder.LayoutOrder
	gridLayout.Parent = page

	pages[tabName] = page
	tabButtons[tabName] = btnStroke

	-- Tab Switching Logic
	tabBtn.MouseButton1Click:Connect(function()
		for name, p in pairs(pages) do
			p.Visible = (name == tabName)
			tabButtons[name].Enabled = (name == tabName)
		end
	end)

	return page
end

-- Create the 3 Tabs
local mainPage = createTab("Main", 1)
local miscPage = createTab("Misc", 2)
local settingsPage = createTab("Settings", 3)

-- Helper Function to Add Action Cards
local function addActionButton(parentPage, title, subtitle, callback)
	local card = Instance.new("TextButton")
	card.BackgroundColor3 = COLORS.Card
	card.Text = ""
	card.AutoButtonColor = false
	card.Parent = parentPage

	local corner = Instance.new("UICorner")
	corner.CornerRadius = UDim.new(0, 8)
	corner.Parent = card

	local stroke = Instance.new("UIStroke")
	stroke.Color = COLORS.GoldAccent
	stroke.Thickness = 1
	stroke.Transparency = 0.5
	stroke.Parent = card

	local titleLbl = Instance.new("TextLabel")
	titleLbl.Size = UDim2.new(1, -20, 0, 24)
	titleLbl.Position = UDim2.new(0, 12, 0, 10)
	titleLbl.BackgroundTransparency = 1
	titleLbl.Text = title
	titleLbl.TextColor3 = COLORS.TextPrimary
	titleLbl.Font = Enum.Font.GothamBold
	titleLbl.TextSize = 14
	titleLbl.TextXAlignment = Enum.TextXAlignment.Left
	titleLbl.Parent = card

	local subLbl = Instance.new("TextLabel")
	subLbl.Size = UDim2.new(1, -20, 0, 20)
	subLbl.Position = UDim2.new(0, 12, 0, 36)
	subLbl.BackgroundTransparency = 1
	subLbl.Text = subtitle
	subLbl.TextColor3 = COLORS.TextSecondary
	subLbl.Font = Enum.Font.Gotham
	subLbl.TextSize = 12
	subLbl.TextXAlignment = Enum.TextXAlignment.Left
	subLbl.Parent = card

	-- Hover Effect
	card.MouseEnter:Connect(function()
		TweenService:Create(card, TweenInfo.new(0.15), {BackgroundColor3 = COLORS.CardHover}):Play()
		stroke.Transparency = 0
	end)

	card.MouseLeave:Connect(function()
		TweenService:Create(card, TweenInfo.new(0.15), {BackgroundColor3 = COLORS.Card}):Play()
		stroke.Transparency = 0.5
	end)

	card.MouseButton1Click:Connect(function()
		if callback then
			callback()
		end
	end)
end

-- Populate 10 Legitimate Game Buttons in "Main"
local mainActions = {
	{"Play Match", "Join active game queue"},
	{"Daily Rewards", "Claim your login bonus"},
	{"Inventory", "View unlocked items & skins"},
	{"Shop", "Browse game passes & boosts"},
	{"Quests", "Track active missions"},
	{"Leaderboard", "View top server players"},
	{"Codes", "Redeem promotional codes"},
	{"Badges", "Check earned achievements"},
	{"Trading", "Open player trade menu"},
	{"Respawn", "Reset character to spawn"}
}

for _, info in ipairs(mainActions) do
	addActionButton(mainPage, info[1], info[2], function()
		print(info[1] .. " clicked!")
		-- Add your legitimate RemoteEvent or UI logic here
	end)
end

-- Example Buttons for Misc and Settings
addActionButton(miscPage, "Server Info", "View server uptime & ping", function()
	print("Server Info opened")
end)

addActionButton(settingsPage, "Toggle Music", "Mute or unmute background audio", function()
	print("Music toggled")
end)
