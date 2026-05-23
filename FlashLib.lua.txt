--// Flash UI Library - Roblox Edition
--// Inspired by Flash_UI from GitHub

local FlashLib = {
	Options = {},
	Folder = "FlashLib",
	GetService = function(service)
		return cloneref and cloneref(game:GetService(service)) or game:GetService(service)
	end
}

--// Services
local TweenService = FlashLib.GetService("TweenService")
local RunService = FlashLib.GetService("RunService")
local UserInputService = FlashLib.GetService("UserInputService")
local Players = FlashLib.GetService("Players")

--// Variables
local LocalPlayer = Players.LocalPlayer
local LocalPlayerGui = LocalPlayer:WaitForChild("PlayerGui")

--// Color Palette
local Colors = {
	Background = Color3.fromRGB(15, 15, 15),
	Surface = Color3.fromRGB(30, 41, 59),
	Border = Color3.fromRGB(87, 86, 86),
	Text = Color3.fromRGB(255, 255, 255),
	TextMuted = Color3.fromRGB(200, 200, 200),
	Accent = Color3.fromRGB(59, 130, 246),
	AccentDark = Color3.fromRGB(30, 58, 138),
	Red = Color3.fromRGB(250, 93, 86),
	Yellow = Color3.fromRGB(252, 190, 57),
	Green = Color3.fromRGB(119, 174, 94),
}

--// Helper Functions
local function Tween(instance, tweeninfo, properties)
	return TweenService:Create(instance, tweeninfo, properties)
end

local function CreateCorner(parent, radius)
	local corner = Instance.new("UICorner")
	corner.CornerRadius = radius or UDim.new(0, 10)
	corner.Parent = parent
	return corner
end

local function CreateStroke(parent, color, transparency)
	local stroke = Instance.new("UIStroke")
	stroke.Color = color or Colors.Border
	stroke.Transparency = transparency or 0.9
	stroke.Parent = parent
	return stroke
end

--// Main Window Function
function FlashLib:Window(Settings)
	local WindowFunctions = {Settings = Settings}
	
	--// Create Main GUI
	local screenGui = Instance.new("ScreenGui")
	screenGui.Name = Settings.Title or "FlashLib Window"
	screenGui.ResetOnSpawn = false
	screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
	screenGui.DisplayOrder = 2147483647
	screenGui.Parent = LocalPlayerGui
	
	--// Create Base Frame
	local base = Instance.new("Frame")
	base.Name = "Base"
	base.AnchorPoint = Vector2.new(0.5, 0.5)
	base.BackgroundColor3 = Colors.Background
	base.BackgroundTransparency = 0.05
	base.BorderSizePixel = 0
	base.Position = UDim2.fromScale(0.5, 0.5)
	base.Size = Settings.Size or UDim2.fromOffset(868, 650)
	base.Parent = screenGui
	
	CreateCorner(base, UDim.new(0, 10))
	CreateStroke(base, Colors.Border, 0.9)
	
	--// Create Sidebar
	local sidebar = Instance.new("Frame")
	sidebar.Name = "Sidebar"
	sidebar.BackgroundTransparency = 1
	sidebar.BorderSizePixel = 0
	sidebar.Size = UDim2.fromScale(0.325, 1)
	sidebar.Parent = base
	
	--// Sidebar Divider
	local divider = Instance.new("Frame")
	divider.Name = "Divider"
	divider.AnchorPoint = Vector2.new(1, 0)
	divider.BackgroundColor3 = Colors.Border
	divider.BackgroundTransparency = 0.9
	divider.BorderSizePixel = 0
	divider.Position = UDim2.fromScale(1, 0)
	divider.Size = UDim2.new(0, 1, 1, 0)
	divider.Parent = sidebar
	
	--// Window Control Buttons
	local windowControls = Instance.new("Frame")
	windowControls.Name = "WindowControls"
	windowControls.BackgroundTransparency = 1
	windowControls.BorderSizePixel = 0
	windowControls.Size = UDim2.new(1, 0, 0, 31)
	windowControls.Parent = sidebar
	
	local controls = Instance.new("Frame")
	controls.Name = "Controls"
	controls.BackgroundTransparency = 1
	controls.BorderSizePixel = 0
	controls.Size = UDim2.fromScale(1, 1)
	controls.Parent = windowControls
	
	local controlLayout = Instance.new("UIListLayout")
	controlLayout.Padding = UDim.new(0, 5)
	controlLayout.FillDirection = Enum.FillDirection.Horizontal
	controlLayout.VerticalAlignment = Enum.VerticalAlignment.Center
	controlLayout.SortOrder = Enum.SortOrder.LayoutOrder
	controlLayout.Parent = controls
	
	local controlPadding = Instance.new("UIPadding")
	controlPadding.PaddingLeft = UDim.new(0, 11)
	controlPadding.Parent = controls
	
	--// Control Buttons
	local function CreateControlButton(name, color)
		local button = Instance.new("TextButton")
		button.Name = name
		button.BackgroundColor3 = color
		button.BorderSizePixel = 0
		button.Size = UDim2.fromOffset(8, 8)
		button.Text = ""
		button.AutoButtonColor = false
		button.Parent = controls
		CreateCorner(button, UDim.new(1, 0))
		return button
	end
	
	local exitButton = CreateControlButton("Exit", Colors.Red)
	local minimizeButton = CreateControlButton("Minimize", Colors.Yellow)
	local maximizeButton = CreateControlButton("Maximize", Colors.Green)
	
	--// Title Section
	local information = Instance.new("Frame")
	information.Name = "Information"
	information.BackgroundTransparency = 1
	information.BorderSizePixel = 0
	information.Position = UDim2.fromOffset(0, 31)
	information.Size = UDim2.new(1, 0, 0, 60)
	information.Parent = sidebar
	
	local infoHolder = Instance.new("Frame")
	infoHolder.Name = "InformationHolder"
	infoHolder.BackgroundTransparency = 1
	infoHolder.BorderSizePixel = 0
	infoHolder.Size = UDim2.fromScale(1, 1)
	infoHolder.Parent = information
	
	local infoPadding = Instance.new("UIPadding")
	infoPadding.PaddingLeft = UDim.new(0, 23)
	infoPadding.PaddingRight = UDim.new(0, 22)
	infoPadding.PaddingTop = UDim.new(0, 10)
	infoPadding.PaddingBottom = UDim.new(0, 10)
	infoPadding.Parent = infoHolder
	
	local titleLabel = Instance.new("TextLabel")
	titleLabel.Name = "Title"
	titleLabel.BackgroundTransparency = 1
	titleLabel.BorderSizePixel = 0
	titleLabel.Font = Enum.Font.GothamBold
	titleLabel.TextColor3 = Colors.Text
	titleLabel.TextSize = 18
	titleLabel.TextTransparency = 0.1
	titleLabel.TextXAlignment = Enum.TextXAlignment.Left
	titleLabel.Text = Settings.Title or "Window"
	titleLabel.Size = UDim2.new(1, -20, 0, 20)
	titleLabel.Parent = infoHolder
	
	local subtitleLabel = Instance.new("TextLabel")
	subtitleLabel.Name = "Subtitle"
	subtitleLabel.BackgroundTransparency = 1
	subtitleLabel.BorderSizePixel = 0
	subtitleLabel.Font = Enum.Font.Gotham
	subtitleLabel.TextColor3 = Colors.Text
	subtitleLabel.TextSize = 12
	subtitleLabel.TextTransparency = 0.7
	subtitleLabel.TextXAlignment = Enum.TextXAlignment.Left
	subtitleLabel.Text = Settings.Subtitle or ""
	subtitleLabel.Size = UDim2.new(1, -20, 0, 15)
	subtitleLabel.Position = UDim2.fromOffset(0, 22)
	subtitleLabel.Parent = infoHolder
	
	--// Sidebar Group
	local sidebarGroup = Instance.new("Frame")
	sidebarGroup.Name = "SidebarGroup"
	sidebarGroup.BackgroundTransparency = 1
	sidebarGroup.BorderSizePixel = 0
	sidebarGroup.Position = UDim2.fromOffset(0, 91)
	sidebarGroup.Size = UDim2.new(1, 0, 1, -91)
	sidebarGroup.Parent = sidebar
	
	local groupPadding = Instance.new("UIPadding")
	groupPadding.PaddingTop = UDim.new(0, 31)
	groupPadding.PaddingLeft = UDim.new(0, 10)
	groupPadding.PaddingRight = UDim.new(0, 10)
	groupPadding.Parent = sidebarGroup
	
	local tabSwitchers = Instance.new("ScrollingFrame")
	tabSwitchers.Name = "TabSwitchers"
	tabSwitchers.BackgroundTransparency = 1
	tabSwitchers.BorderSizePixel = 0
	tabSwitchers.AutomaticCanvasSize = Enum.AutomaticSize.Y
	tabSwitchers.ScrollBarThickness = 1
	tabSwitchers.CanvasSize = UDim2.new()
	tabSwitchers.Size = UDim2.fromScale(1, 1)
	tabSwitchers.Parent = sidebarGroup
	
	local tabLayout = Instance.new("UIListLayout")
	tabLayout.Padding = UDim.new(0, 17)
	tabLayout.SortOrder = Enum.SortOrder.LayoutOrder
	tabLayout.Parent = tabSwitchers
	
	local tabPadding = Instance.new("UIPadding")
	tabPadding.PaddingTop = UDim.new(0, 2)
	tabPadding.Parent = tabSwitchers
	
	--// Content Area
	local content = Instance.new("Frame")
	content.Name = "Content"
	content.AnchorPoint = Vector2.new(1, 0)
	content.BackgroundTransparency = 1
	content.BorderSizePixel = 0
	content.Position = UDim2.fromScale(1, 0)
	content.Size = UDim2.new(0.675, 0, 1, 0)
	content.Parent = base
	
	--// Topbar
	local topbar = Instance.new("Frame")
	topbar.Name = "Topbar"
	topbar.BackgroundTransparency = 1
	topbar.BorderSizePixel = 0
	topbar.Size = UDim2.new(1, 0, 0, 63)
	topbar.Parent = content
	
	local topbarDivider = Instance.new("Frame")
	topbarDivider.Name = "Divider"
	topbarDivider.AnchorPoint = Vector2.new(0, 1)
	topbarDivider.BackgroundColor3 = Colors.Border
	topbarDivider.BackgroundTransparency = 0.9
	topbarDivider.BorderSizePixel = 0
	topbarDivider.Position = UDim2.fromScale(0, 1)
	topbarDivider.Size = UDim2.new(1, 0, 0, 1)
	topbarDivider.Parent = topbar
	
	local topbarElements = Instance.new("Frame")
	topbarElements.Name = "Elements"
	topbarElements.BackgroundTransparency = 1
	topbarElements.BorderSizePixel = 0
	topbarElements.Size = UDim2.fromScale(1, 1)
	topbarElements.Parent = topbar
	
	local topbarPadding = Instance.new("UIPadding")
	topbarPadding.PaddingLeft = UDim.new(0, 20)
	topbarPadding.PaddingRight = UDim.new(0, 20)
	topbarPadding.Parent = topbarElements
	
	local currentTabLabel = Instance.new("TextLabel")
	currentTabLabel.Name = "CurrentTab"
	currentTabLabel.BackgroundTransparency = 1
	currentTabLabel.BorderSizePixel = 0
	currentTabLabel.Font = Enum.Font.Gotham
	currentTabLabel.TextColor3 = Colors.Text
	currentTabLabel.TextSize = 15
	currentTabLabel.TextTransparency = 0.5
	currentTabLabel.TextXAlignment = Enum.TextXAlignment.Left
	currentTabLabel.AnchorPoint = Vector2.new(0, 0.5)
	currentTabLabel.Position = UDim2.fromScale(0, 0.5)
	currentTabLabel.Size = UDim2.fromScale(0.9, 0)
	currentTabLabel.AutomaticSize = Enum.AutomaticSize.Y
	currentTabLabel.Parent = topbarElements
	
	--// Tab Management
	local tabs = {}
	local currentTab = nil
	local tabIndex = 0
	
	function WindowFunctions:AddTab(tabName, imageName)
		tabIndex = tabIndex + 1
		
		--// Tab Button
		local tabButton = Instance.new("TextButton")
		tabButton.Name = tabName
		tabButton.BackgroundColor3 = Colors.Surface
		tabButton.BorderSizePixel = 0
		tabButton.Size = UDim2.new(1, -21, 0, 40)
		tabButton.Font = Enum.Font.Gotham
		tabButton.TextColor3 = Colors.TextMuted
		tabButton.TextSize = 16
		tabButton.Text = tabName
		tabButton.TextTransparency = 0.5
		tabButton.AutoButtonColor = false
		tabButton.LayoutOrder = tabIndex
		tabButton.Parent = tabSwitchers
		
		CreateCorner(tabButton, UDim.new(0, 5))
		CreateStroke(tabButton, Colors.Border, 1)
		
		--// Tab Content
		local tabContent = Instance.new("ScrollingFrame")
		tabContent.Name = tabName .. "Content"
		tabContent.BackgroundTransparency = 1
		tabContent.BorderSizePixel = 0
		tabContent.AutomaticCanvasSize = Enum.AutomaticSize.Y
		tabContent.ScrollBarThickness = 1
		tabContent.CanvasSize = UDim2.new()
		tabContent.Size = UDim2.new(0.5, 0, 1, -63)
		tabContent.Position = UDim2.fromOffset(0, 63)
		tabContent.ClipsDescendants = true
		tabContent.Visible = (tabIndex == 1)
		tabContent.Parent = content
		
		local contentLayout = Instance.new("UIListLayout")
		contentLayout.Padding = UDim.new(0, 15)
		contentLayout.SortOrder = Enum.SortOrder.LayoutOrder
		contentLayout.Parent = tabContent
		
		local contentPadding = Instance.new("UIPadding")
		contentPadding.PaddingLeft = UDim.new(0, 11)
		contentPadding.PaddingRight = UDim.new(0, 3)
		contentPadding.PaddingTop = UDim.new(0, 10)
		contentPadding.PaddingBottom = UDim.new(0, 10)
		contentPadding.Parent = tabContent
		
		--// Tab Click Handler
		tabButton.MouseButton1Click:Connect(function()
			--// Hide all tabs
			for _, tab in ipairs(tabs) do
				tab.Content.Visible = false
				tab.Button.TextTransparency = 0.5
				tab.Button.TextColor3 = Colors.TextMuted
			end
			
			--// Show current tab
			tabContent.Visible = true
			tabButton.TextTransparency = 0.1
			tabButton.TextColor3 = Colors.Text
			currentTab = tabName
			currentTabLabel.Text = tabName
		end)
		
		if tabIndex == 1 then
			tabButton.TextTransparency = 0.1
			tabButton.TextColor3 = Colors.Text
			currentTabLabel.Text = tabName
		end
		
		local tabObj = {
			Name = tabName,
			Button = tabButton,
			Content = tabContent,
			Layout = contentLayout,
			AddSection = function(self)
				return WindowFunctions:AddSection(tabContent)
			end,
			AddButton = function(self, name, callback)
				return WindowFunctions:AddButton(tabContent, name, callback)
			end,
			AddToggle = function(self, name, callback)
				return WindowFunctions:AddToggle(tabContent, name, callback)
			end,
			AddSlider = function(self, name, min, max, default, callback)
				return WindowFunctions:AddSlider(tabContent, name, min, max, default, callback)
			end,
			AddDropdown = function(self, name, options, callback)
				return WindowFunctions:AddDropdown(tabContent, name, options, callback)
			end,
			AddInput = function(self, name, placeholder, callback)
				return WindowFunctions:AddInput(tabContent, name, placeholder, callback)
			end
		}
		
		table.insert(tabs, tabObj)
		return tabObj
	end
	
	--// Section Function
	function WindowFunctions:AddSection(parent)
		local section = Instance.new("Frame")
		section.Name = "Section"
		section.AutomaticSize = Enum.AutomaticSize.Y
		section.BackgroundColor3 = Colors.Surface
		section.BackgroundTransparency = 0.98
		section.BorderSizePixel = 0
		section.Size = UDim2.fromScale(1, 0)
		section.Parent = parent
		
		CreateCorner(section, UDim.new(0, 5))
		CreateStroke(section, Colors.Border, 0.95)
		
		local sectionLayout = Instance.new("UIListLayout")
		sectionLayout.Padding = UDim.new(0, 10)
		sectionLayout.SortOrder = Enum.SortOrder.LayoutOrder
		sectionLayout.Parent = section
		
		local sectionPadding = Instance.new("UIPadding")
		sectionPadding.PaddingTop = UDim.new(0, 15)
		sectionPadding.PaddingBottom = UDim.new(0, 15)
		sectionPadding.PaddingLeft = UDim.new(0, 15)
		sectionPadding.PaddingRight = UDim.new(0, 15)
		sectionPadding.Parent = section
		
		return {
			Frame = section,
			AddButton = function(self, name, callback)
				return WindowFunctions:AddButton(section, name, callback)
			end,
			AddToggle = function(self, name, callback)
				return WindowFunctions:AddToggle(section, name, callback)
			end,
			AddSlider = function(self, name, min, max, default, callback)
				return WindowFunctions:AddSlider(section, name, min, max, default, callback)
			end,
			AddLabel = function(self, text)
				local label = Instance.new("TextLabel")
				label.BackgroundTransparency = 1
				label.BorderSizePixel = 0
				label.Size = UDim2.new(1, 0, 0, 20)
				label.Font = Enum.Font.Gotham
				label.TextColor3 = Colors.TextMuted
				label.TextSize = 12
				label.Text = text
				label.TextXAlignment = Enum.TextXAlignment.Left
				label.Parent = section
				return label
			end
		}
	end
	
	--// Button Function
	function WindowFunctions:AddButton(parent, buttonName, callback)
		local buttonFrame = Instance.new("Frame")
		buttonFrame.BackgroundTransparency = 1
		buttonFrame.BorderSizePixel = 0
		buttonFrame.Size = UDim2.new(1, 0, 0, 32)
		buttonFrame.Parent = parent
		
		local button = Instance.new("TextButton")
		button.Name = buttonName
		button.BackgroundColor3 = Colors.Border
		button.BorderSizePixel = 0
		button.Size = UDim2.fromScale(1, 1)
		button.Font = Enum.Font.Gotham
		button.TextColor3 = Colors.Text
		button.TextSize = 13
		button.Text = buttonName
		button.AutoButtonColor = false
		button.TextTransparency = 0.5
		button.Parent = buttonFrame
		
		CreateCorner(button, UDim.new(0, 5))
		
		--// Hover Effects
		button.MouseEnter:Connect(function()
			Tween(button, TweenInfo.new(0.2), {BackgroundColor3 = Colors.Accent, TextTransparency = 0.3}):Play()
		end)
		
		button.MouseLeave:Connect(function()
			Tween(button, TweenInfo.new(0.2), {BackgroundColor3 = Colors.Border, TextTransparency = 0.5}):Play()
		end)
		
		button.MouseButton1Click:Connect(function()
			if callback then callback() end
		end)
		
		return button
	end
	
	--// Toggle Function
	function WindowFunctions:AddToggle(parent, toggleName, callback)
		local toggleFrame = Instance.new("Frame")
		toggleFrame.BackgroundTransparency = 1
		toggleFrame.BorderSizePixel = 0
		toggleFrame.Size = UDim2.new(1, 0, 0, 32)
		toggleFrame.Parent = parent
		
		local label = Instance.new("TextLabel")
		label.BackgroundTransparency = 1
		label.BorderSizePixel = 0
		label.Size = UDim2.new(0.65, 0, 1, 0)
		label.Font = Enum.Font.Gotham
		label.TextColor3 = Colors.TextMuted
		label.TextSize = 13
		label.Text = toggleName
		label.TextXAlignment = Enum.TextXAlignment.Left
		label.TextTransparency = 0.5
		label.Parent = toggleFrame
		
		local toggleBg = Instance.new("Frame")
		toggleBg.BackgroundColor3 = Colors.Border
		toggleBg.BorderSizePixel = 0
		toggleBg.Size = UDim2.new(0, 45, 0, 24)
		toggleBg.Position = UDim2.new(0.65, 10, 0.25, 0)
		toggleBg.Parent = toggleFrame
		CreateCorner(toggleBg, UDim.new(0, 12))
		
		local toggleButton = Instance.new("TextButton")
		toggleButton.BackgroundColor3 = Colors.Border
		toggleButton.BorderSizePixel = 0
		toggleButton.Size = UDim2.new(0, 20, 0, 20)
		toggleButton.Position = UDim2.new(0, 2, 0.5, -10)
		toggleButton.Text = ""
		toggleButton.AutoButtonColor = false
		toggleButton.Parent = toggleBg
		CreateCorner(toggleButton, UDim.new(0, 10))
		
		local state = false
		
		toggleButton.MouseButton1Click:Connect(function()
			state = not state
			local newPos = state and UDim2.new(0, 23, 0.5, -10) or UDim2.new(0, 2, 0.5, -10)
			local newColor = state and Colors.Accent or Colors.Border
			
			Tween(toggleButton, TweenInfo.new(0.2), {Position = newPos, BackgroundColor3 = newColor}):Play()
			
			if callback then callback(state) end
		end)
		
		return {
			Value = state,
			GetValue = function() return state end,
			SetValue = function(self, val)
				state = val
				local newPos = state and UDim2.new(0, 23, 0.5, -10) or UDim2.new(0, 2, 0.5, -10)
				local newColor = state and Colors.Accent or Colors.Border
				toggleButton.Position = newPos
				toggleButton.BackgroundColor3 = newColor
				if callback then callback(state) end
			end
		}
	end
	
	--// Slider Function
	function WindowFunctions:AddSlider(parent, sliderName, min, max, default, callback)
		local sliderFrame = Instance.new("Frame")
		sliderFrame.BackgroundTransparency = 1
		sliderFrame.BorderSizePixel = 0
		sliderFrame.Size = UDim2.new(1, 0, 0, 50)
		sliderFrame.Parent = parent
		
		local label = Instance.new("TextLabel")
		label.BackgroundTransparency = 1
		label.BorderSizePixel = 0
		label.Size = UDim2.new(1, 0, 0, 20)
		label.Font = Enum.Font.Gotham
		label.TextColor3 = Colors.TextMuted
		label.TextSize = 13
		label.Text = sliderName .. ": " .. tostring(default or min)
		label.TextXAlignment = Enum.TextXAlignment.Left
		label.TextTransparency = 0.5
		label.Parent = sliderFrame
		
		local sliderBg = Instance.new("Frame")
		sliderBg.BackgroundColor3 = Colors.Border
		sliderBg.BorderSizePixel = 0
		sliderBg.Size = UDim2.new(1, 0, 0, 6)
		sliderBg.Position = UDim2.new(0, 0, 0.5, 3)
		sliderBg.Parent = sliderFrame
		CreateCorner(sliderBg, UDim.new(0, 3))
		
		local sliderValue = Instance.new("Frame")
		sliderValue.BackgroundColor3 = Colors.Accent
		sliderValue.BorderSizePixel = 0
		sliderValue.Size = UDim2.new(0, 0, 1, 0)
		sliderValue.Parent = sliderBg
		CreateCorner(sliderValue, UDim.new(0, 3))
		
		local sliderButton = Instance.new("TextButton")
		sliderButton.BackgroundColor3 = Colors.Accent
		sliderButton.BorderSizePixel = 0
		sliderButton.Size = UDim2.new(0, 12, 0, 12)
		sliderButton.Position = UDim2.new(0, -6, 0.5, -6)
		sliderButton.Text = ""
		sliderButton.AutoButtonColor = false
		sliderButton.Parent = sliderBg
		CreateCorner(sliderButton, UDim.new(0, 6))
		
		local value = default or min
		local dragging = false
		
		local function updateSlider(newValue)
			value = math.clamp(newValue, min, max)
			local percentage = (value - min) / (max - min)
			sliderValue.Size = UDim2.new(percentage, 0, 1, 0)
			sliderButton.Position = UDim2.new(percentage, -6, 0.5, -6)
			label.Text = sliderName .. ": " .. tostring(math.floor(value))
			if callback then callback(value) end
		end
		
		sliderButton.MouseButton1Down:Connect(function()
			dragging = true
		end)
		
		UserInputService.InputEnded:Connect(function(input, gameProcessed)
			if input.UserInputType == Enum.UserInputType.MouseButton1 then
				dragging = false
			end
		end)
		
		UserInputService.InputChanged:Connect(function(input, gameProcessed)
			if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
				local mousePos = UserInputService:GetMouseLocation()
				local sliderPos = sliderBg.AbsolutePosition.X
				local sliderSize = sliderBg.AbsoluteSize.X
				local relativePos = math.clamp(mousePos.X - sliderPos, 0, sliderSize)
				local percentage = relativePos / sliderSize
				updateSlider(min + (max - min) * percentage)
			end
		end)
		
		sliderBg.MouseButton1Click:Connect(function()
			local mousePos = UserInputService:GetMouseLocation()
			local sliderPos = sliderBg.AbsolutePosition.X
			local sliderSize = sliderBg.AbsoluteSize.X
			local relativePos = math.clamp(mousePos.X - sliderPos, 0, sliderSize)
			local percentage = relativePos / sliderSize
			updateSlider(min + (max - min) * percentage)
		end)
		
		updateSlider(value)
		
		return {
			Value = value,
			GetValue = function() return value end,
			SetValue = function(self, newVal)
				updateSlider(newVal)
			end
		}
	end
	
	--// Dropdown Function
	function WindowFunctions:AddDropdown(parent, dropdownName, options, callback)
		local dropdownFrame = Instance.new("Frame")
		dropdownFrame.BackgroundTransparency = 1
		dropdownFrame.BorderSizePixel = 0
		dropdownFrame.Size = UDim2.new(1, 0, 0, 32)
		dropdownFrame.Parent = parent
		
		local dropdownButton = Instance.new("TextButton")
		dropdownButton.BackgroundColor3 = Colors.Border
		dropdownButton.BorderSizePixel = 0
		dropdownButton.Size = UDim2.fromScale(1, 1)
		dropdownButton.Font = Enum.Font.Gotham
		dropdownButton.TextColor3 = Colors.Text
		dropdownButton.TextSize = 13
		dropdownButton.Text = options[1] or "Select..."
		dropdownButton.AutoButtonColor = false
		dropdownButton.TextTransparency = 0.5
		dropdownButton.Parent = dropdownFrame
		CreateCorner(dropdownButton, UDim.new(0, 5))
		
		local dropdownList = Instance.new("Frame")
		dropdownList.Name = "DropdownList"
		dropdownList.BackgroundColor3 = Colors.Surface
		dropdownList.BorderSizePixel = 0
		dropdownList.Size = UDim2.fromScale(1, 0)
		dropdownList.Position = UDim2.fromScale(0, 1)
		dropdownList.Visible = false
		dropdownList.Parent = dropdownFrame
		dropdownList.ZIndex = 10
		CreateCorner(dropdownList, UDim.new(0, 5))
		
		local listLayout = Instance.new("UIListLayout")
		listLayout.Padding = UDim.new(0, 0)
		listLayout.SortOrder = Enum.SortOrder.LayoutOrder
		listLayout.Parent = dropdownList
		
		local selectedOption = options[1] or ""
		local isOpen = false
		
		--// Populate Options
		for i, option in ipairs(options) do
			local optionButton = Instance.new("TextButton")
			optionButton.BackgroundColor3 = Colors.Surface
			optionButton.BorderSizePixel = 0
			optionButton.Size = UDim2.new(1, 0, 0, 28)
			optionButton.Font = Enum.Font.Gotham
			optionButton.TextColor3 = Colors.Text
			optionButton.TextSize = 13
			optionButton.Text = option
			optionButton.AutoButtonColor = false
			optionButton.TextTransparency = 0.5
			optionButton.Parent = dropdownList
			
			optionButton.MouseButton1Click:Connect(function()
				selectedOption = option
				dropdownButton.Text = option
				dropdownList.Visible = false
				isOpen = false
				if callback then callback(option) end
			end)
			
			optionButton.MouseEnter:Connect(function()
				Tween(optionButton, TweenInfo.new(0.1), {BackgroundColor3 = Colors.Accent}):Play()
			end)
			
			optionButton.MouseLeave:Connect(function()
				Tween(optionButton, TweenInfo.new(0.1), {BackgroundColor3 = Colors.Surface}):Play()
			end)
		end
		
		dropdownButton.MouseButton1Click:Connect(function()
			isOpen = not isOpen
			dropdownList.Visible = isOpen
			local newSize = isOpen and UDim2.new(1, 0, 0, #options * 28) or UDim2.new(1, 0, 0, 0)
			Tween(dropdownList, TweenInfo.new(0.2), {Size = newSize}):Play()
		end)
		
		return {
			GetValue = function() return selectedOption end,
			SetValue = function(self, val)
				selectedOption = val
				dropdownButton.Text = val
				if callback then callback(val) end
			end
		}
	end
	
	--// Input Function
	function WindowFunctions:AddInput(parent, inputName, placeholder, callback)
		local inputFrame = Instance.new("Frame")
		inputFrame.BackgroundTransparency = 1
		inputFrame.BorderSizePixel = 0
		inputFrame.Size = UDim2.new(1, 0, 0, 32)
		inputFrame.Parent = parent
		
		local label = Instance.new("TextLabel")
		label.BackgroundTransparency = 1
		label.BorderSizePixel = 0
		label.Size = UDim2.new(0.35, 0, 1, 0)
		label.Font = Enum.Font.Gotham
		label.TextColor3 = Colors.TextMuted
		label.TextSize = 13
		label.Text = inputName
		label.TextXAlignment = Enum.TextXAlignment.Left
		label.TextTransparency = 0.5
		label.Parent = inputFrame
		
		local textBox = Instance.new("TextBox")
		textBox.BackgroundColor3 = Colors.Border
		textBox.BorderSizePixel = 0
		textBox.Size = UDim2.new(0.65, 0, 1, 0)
		textBox.Position = UDim2.new(0.35, 5, 0, 0)
		textBox.Font = Enum.Font.Gotham
		textBox.TextColor3 = Colors.Text
		textBox.TextSize = 13
		textBox.PlaceholderColor3 = Colors.TextMuted
		textBox.PlaceholderText = placeholder or "Enter text..."
		textBox.ClearTextOnFocus = false
		textBox.TextTransparency = 0.1
		textBox.Parent = inputFrame
		CreateCorner(textBox, UDim.new(0, 5))
		
		textBox.FocusLost:Connect(function()
			if callback then callback(textBox.Text) end
		end)
		
		return {
			GetValue = function() return textBox.Text end,
			SetValue = function(self, val)
				textBox.Text = val
			end
		}
	end
	
	--// Notification Function
	function WindowFunctions:Notify(message, duration)
		local notification = Instance.new("Frame")
		notification.BackgroundColor3 = Colors.Surface
		notification.BorderSizePixel = 0
		notification.Size = UDim2.new(0, 300, 0, 60)
		notification.Position = UDim2.new(1, -320, 0.95, -80)
		notification.Parent = screenGui
		CreateCorner(notification, UDim.new(0, 8))
		CreateStroke(notification, Colors.Border, 0.9)
		
		local textLabel = Instance.new("TextLabel")
		textLabel.BackgroundTransparency = 1
		textLabel.BorderSizePixel = 0
		textLabel.Size = UDim2.fromScale(1, 1)
		textLabel.Font = Enum.Font.Gotham
		textLabel.TextColor3 = Colors.Text
		textLabel.TextSize = 12
		textLabel.Text = message
		textLabel.TextWrapped = true
		textLabel.TextTransparency = 0.2
		textLabel.Parent = notification
		
		local padding = Instance.new("UIPadding")
		padding.PaddingTop = UDim.new(0, 8)
		padding.PaddingBottom = UDim.new(0, 8)
		padding.PaddingLeft = UDim.new(0, 12)
		padding.PaddingRight = UDim.new(0, 12)
		padding.Parent = notification
		
		if duration then
			task.wait(duration)
			notification:Destroy()
		end
		
		return notification
	end
	
	--// Close Handler
	exitButton.MouseButton1Click:Connect(function()
		screenGui:Destroy()
	end)
	
	--// Minimize Handler
	minimizeButton.MouseButton1Click:Connect(function()
		content.Visible = not content.Visible
	end)
	
	return WindowFunctions
end

return FlashLib
