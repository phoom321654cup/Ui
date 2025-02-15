local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui")
gui.Parent = player:WaitForChild("PlayerGui")
gui.Name = "CustomUI"
gui.ResetOnSpawn = false

-- Main Frame
local mainFrame = Instance.new("Frame")
mainFrame.Parent = gui
mainFrame.Size = UDim2.new(0, 400, 0, 300)
mainFrame.Position = UDim2.new(0.5, -200, 0.5, -150)
mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
mainFrame.BorderSizePixel = 0
mainFrame.Active = true

-- UICorner for rounded edges
local corner = Instance.new("UICorner")
corner.Parent = mainFrame
corner.CornerRadius = UDim.new(0, 10)

-- Title (เปลี่ยนชื่อเป็น Life Hub)
local title = Instance.new("TextLabel")
title.Parent = mainFrame
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundTransparency = 1
title.Text = "Life Hub"
title.TextColor3 = Color3.new(1, 1, 1)
title.TextScaled = true
title.Font = Enum.Font.GothamBold
title.Active = true

-- Dragging Functionality (ลากด้วย Title)
local dragging = false
local dragInput, mousePos, framePos

title.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        mousePos = Vector2.new(input.Position.X, input.Position.Y)
        framePos = mainFrame.Position

        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

title.InputChanged:Connect(function(input)
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
    
    if name == "AuToFarm" then
        -- RedeemCode Button
        local redeemButton = Instance.new("TextButton")
        redeemButton.Parent = page
        redeemButton.Size = UDim2.new(0, 200, 0, 40)
        redeemButton.Position = UDim2.new(0, 10, 0, 10)
        redeemButton.Text = "RedeemCode"
        redeemButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
        redeemButton.TextColor3 = Color3.new(1, 1, 1)
        redeemButton.BorderSizePixel = 0
        redeemButton.Font = Enum.Font.Gotham
        redeemButton.TextScaled = true
        
        -- AuToFarm Toggle
        local autoFarmToggle = Instance.new("TextButton")
        autoFarmToggle.Parent = page
        autoFarmToggle.Size = UDim2.new(0, 200, 0, 40)
        autoFarmToggle.Position = UDim2.new(0, 10, 0, 60)
        autoFarmToggle.Text = "AuToFarm: OFF"
        autoFarmToggle.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
        autoFarmToggle.TextColor3 = Color3.new(1, 1, 1)
        autoFarmToggle.BorderSizePixel = 0
        autoFarmToggle.Font = Enum.Font.Gotham
        autoFarmToggle.TextScaled = true
        
        local isAutoFarm = false
        autoFarmToggle.MouseButton1Click:Connect(function()
            isAutoFarm = not isAutoFarm
            autoFarmToggle.Text = isAutoFarm and "AuToFarm: ON" or "AuToFarm: OFF"
        end)
    end
    
    return page
end

-- Create Tabs and Pages
local tabs = {}
local pages = {}
local tabNames = {"AuToFarm"}

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
