local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui")
gui.Parent = player:WaitForChild("PlayerGui")
gui.Name = "CustomUI"
gui.ResetOnSpawn = false

-- Main Frame
local mainFrame = Instance.new("Frame")
mainFrame.Parent = gui
mainFrame.Size = UDim2.new(0, 400, 0, 250)
mainFrame.Position = UDim2.new(0.5, -200, 0.5, -125)
mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
mainFrame.BorderSizePixel = 0

-- UICorner for rounded edges
local corner = Instance.new("UICorner")
corner.Parent = mainFrame
corner.CornerRadius = UDim.new(0, 10)

-- Title
local title = Instance.new("TextLabel")
title.Parent = mainFrame
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundTransparency = 1
title.Text = "Library Title"
title.TextColor3 = Color3.new(1, 1, 1)
title.TextScaled = true
title.Font = Enum.Font.GothamBold

-- Minimize Button
local minimizeButton = Instance.new("TextButton")
minimizeButton.Parent = mainFrame
minimizeButton.Size = UDim2.new(0, 30, 0, 30)
minimizeButton.Position = UDim2.new(1, -35, 0, 5)
minimizeButton.Text = "-"
minimizeButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
minimizeButton.TextColor3 = Color3.new(1, 1, 1)
minimizeButton.BorderSizePixel = 0
minimizeButton.Font = Enum.Font.GothamBold
minimizeButton.TextScaled = true

-- Drag Button (+)
local dragButton = Instance.new("TextButton")
dragButton.Parent = mainFrame
dragButton.Size = UDim2.new(0, 30, 0, 30)
dragButton.Position = UDim2.new(1, -70, 0, 5) -- อยู่ข้างๆ ปุ่ม -
dragButton.Text = "+"
dragButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
dragButton.TextColor3 = Color3.new(1, 1, 1)
dragButton.BorderSizePixel = 0
dragButton.Font = Enum.Font.GothamBold
dragButton.TextScaled = true

-- Dragging Functionality
local dragging = false
local dragInput, mousePos, framePos

dragButton.MouseButton1Down:Connect(function()
    dragging = true
    mousePos = Vector2.new(game.Players.LocalPlayer:GetMouse().X, game.Players.LocalPlayer:GetMouse().Y)
    framePos = mainFrame.Position

    local inputChanged
    inputChanged = game:GetService("UserInputService").InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement and dragging then
            local delta = Vector2.new(input.Position.X, input.Position.Y) - mousePos
            mainFrame.Position = UDim2.new(
                framePos.X.Scale,
                framePos.X.Offset + delta.X,
                framePos.Y.Scale,
                framePos.Y.Offset + delta.Y
            )
        end
    end)

    -- เมื่อปล่อยเม้าส์ให้หยุดลาก
    game:GetService("UserInputService").InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = false
            inputChanged:Disconnect()
        end
    end)
end)

-- Sidebar
local sidebar = Instance.new("Frame")
sidebar.Parent = mainFrame
sidebar.Size = UDim2.new(0, 80, 1, -40)
sidebar.Position = UDim2.new(0, 0, 0, 40)
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
contentArea.Size = UDim2.new(1, -80, 1, -40)
contentArea.Position = UDim2.new(0, 80, 0, 40)
contentArea.BackgroundTransparency = 1

-- Function for Creating Tabs
local function createTabButton(name)
    local tabButton = Instance.new("TextButton")
    tabButton.Size = UDim2.new(1, 0, 0, 40)
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
    
    -- Button Example
    local button = Instance.new("TextButton")
    button.Parent = page
    button.Size = UDim2.new(1, -20, 0, 40)
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

-- Toggle Visibility
local isVisible = true
local toggleButton = Instance.new("TextButton")
toggleButton.Parent = gui
toggleButton.Size = UDim2.new(0, 50, 0, 30)
toggleButton.Position = UDim2.new(0.5, -25, 0, 10)
toggleButton.Text = "^"
toggleButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
toggleButton.TextColor3 = Color3.new(1, 1, 1)
toggleButton.BorderSizePixel = 0
toggleButton.Font = Enum.Font.GothamBold
toggleButton.TextScaled = true
toggleButton.Visible = false

minimizeButton.MouseButton1Click:Connect(function()
    mainFrame.Visible = false
    toggleButton.Visible = true
end)

toggleButton.MouseButton1Click:Connect(function()
    mainFrame.Visible = true
    toggleButton.Visible = false
end)
