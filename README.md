local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui")
gui.Parent = player:WaitForChild("PlayerGui")
gui.Name = "CustomUI"
gui.ResetOnSpawn = false

-- Main Frame
local mainFrame = Instance.new("Frame")
mainFrame.Parent = gui
mainFrame.Size = UDim2.new(0, 500, 0, 350)
mainFrame.Position = UDim2.new(0.5, -250, 0.5, -175)
mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
mainFrame.BorderSizePixel = 0

-- UICorner for rounded edges
local corner = Instance.new("UICorner")
corner.Parent = mainFrame
corner.CornerRadius = UDim.new(0, 10)

-- Title
local title = Instance.new("TextLabel")
title.Parent = mainFrame
title.Size = UDim2.new(1, 0, 0, 50)
title.BackgroundTransparency = 1
title.Text = "Title of the Library"
title.TextColor3 = Color3.new(1, 1, 1)
title.TextScaled = true
title.Font = Enum.Font.GothamBold

-- Sidebar
local sidebar = Instance.new("Frame")
sidebar.Parent = mainFrame
sidebar.Size = UDim2.new(0, 100, 1, -50)
sidebar.Position = UDim2.new(0, 0, 0, 50)
sidebar.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
sidebar.BorderSizePixel = 0

-- UIListLayout for Sidebar Buttons
local listLayout = Instance.new("UIListLayout")
listLayout.Parent = sidebar
listLayout.SortOrder = Enum.SortOrder.LayoutOrder
listLayout.Padding = UDim.new(0, 5)

-- Content Area
local contentArea = Instance.new("Frame")
contentArea.Parent = mainFrame
contentArea.Size = UDim2.new(1, -100, 1, -50)
contentArea.Position = UDim2.new(0, 100, 0, 50)
contentArea.BackgroundTransparency = 1

-- Function for Creating Tabs
local function createTabButton(name)
    local tabButton = Instance.new("TextButton")
    tabButton.Size = UDim2.new(1, 0, 0, 50)
    tabButton.Text = name
    tabButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    tabButton.TextColor3 = Color3.new(1, 1, 1)
    tabButton.BorderSizePixel = 0
    tabButton.Font = Enum.Font.Gotham
    tabButton.TextScaled = true
    tabButton.Parent = sidebar
    return tabButton
end

-- Function for Creating Content Pages
local function createContentPage(name)
    local page = Instance.new("Frame")
    page.Size = UDim2.new(1, 0, 1, 0)
    page.BackgroundTransparency = 1
    page.Visible = false
    page.Name = name
    page.Parent = contentArea
    
    -- Button
    local button = Instance.new("TextButton")
    button.Parent = page
    button.Size = UDim2.new(1, -20, 0, 50)
    button.Position = UDim2.new(0, 10, 0, 10)
    button.Text = "Button!"
    button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    button.TextColor3 = Color3.new(1, 1, 1)
    button.BorderSizePixel = 0
    button.Font = Enum.Font.Gotham
    button.TextScaled = true
    button.MouseButton1Click:Connect(function()
        print("Button clicked in " .. name)
    end)

    -- Toggle
    local toggle = Instance.new("TextButton")
    toggle.Parent = page
    toggle.Size = UDim2.new(1, -20, 0, 50)
    toggle.Position = UDim2.new(0, 10, 0, 70)
    toggle.Text = "This is a toggle!"
    toggle.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    toggle.TextColor3 = Color3.new(1, 1, 1)
    toggle.BorderSizePixel = 0
    toggle.Font = Enum.Font.Gotham
    toggle.TextScaled = true

    local isToggled = false
    toggle.MouseButton1Click:Connect(function()
        isToggled = not isToggled
        toggle.BackgroundColor3 = isToggled and Color3.new(0, 1, 0) or Color3.fromRGB(50, 50, 50)
        print("Toggle state in " .. name .. ":", isToggled)
    end)

    -- Color Picker
    local colorPicker = Instance.new("TextButton")
    colorPicker.Parent = page
    colorPicker.Size = UDim2.new(0, 50, 0, 50)
    colorPicker.Position = UDim2.new(0, 10, 0, 130)
    colorPicker.BackgroundColor3 = Color3.new(1, 0, 0)
    colorPicker.BorderSizePixel = 0
    colorPicker.Text = ""
    colorPicker.MouseButton1Click:Connect(function()
        colorPicker.BackgroundColor3 = Color3.new(math.random(), math.random(), math.random())
    end)

    -- Slider (แบบง่าย ๆ ด้วย TextBox)
    local slider = Instance.new("TextBox")
    slider.Parent = page
    slider.Size = UDim2.new(1, -20, 0, 50)
    slider.Position = UDim2.new(0, 10, 0, 190)
    slider.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    slider.TextColor3 = Color3.new(1, 1, 1)
    slider.BorderSizePixel = 0
    slider.Font = Enum.Font.Gotham
    slider.TextScaled = true
    slider.Text = "5 bananas"
    slider.FocusLost:Connect(function(enterPressed)
        if enterPressed then
            print("Slider value in " .. name .. ":", slider.Text)
        end
    end)
    
    return page
end

-- Create Tabs and Pages
local tabs = {}
local pages = {}
local tabNames = {"Tab 1", "Tab 2", "Tab 3"}

for _, name in pairs(tabNames) do
    local tab = createTabButton(name)
    local page = createContentPage(name)
    table.insert(tabs, tab)
    table.insert(pages, page)
    
    tab.MouseButton1Click:Connect(function()
        for _, p in pairs(pages) do
            p.Visible = false
        end
        page.Visible = true
        
        for _, t in pairs(tabs) do
            t.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
        end
        tab.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    end)
end

-- เปิดหน้าแรกเป็นค่าเริ่มต้น
pages[1].Visible = true
tabs[1].BackgroundColor3 = Color3.fromRGB(60, 60, 60)
