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
local UserInputService = game:GetService("UserInputService")
local dragging = false
local mousePos, framePos

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

UserInputService.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = Vector2.new(input.Position.X, input.Position.Y) - mousePos
        mainFrame.Position = UDim2.new(
            framePos.X.Scale,
            framePos.X.Offset + delta.X,
            framePos.Y.Scale,
            framePos.Y.Offset + delta.Y
        )
    end
end)

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

-- Toggle Visibility
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

-- Content Area
local contentArea = Instance.new("Frame")
contentArea.Parent = mainFrame
contentArea.Size = UDim2.new(1, -80, 1, -40)
contentArea.Position = UDim2.new(0, 80, 0, 40)
contentArea.BackgroundTransparency = 1

-- Content Area (ScrollingFrame)
local contentArea = Instance.new("ScrollingFrame")
contentArea.Parent = mainFrame
contentArea.Size = UDim2.new(1, -80, 1, -40)
contentArea.Position = UDim2.new(0, 80, 0, 40)
contentArea.BackgroundTransparency = 1
contentArea.CanvasSize = UDim2.new(0, 0, 2, 0) -- ปรับความสูงของเนื้อหา
contentArea.ScrollBarThickness = 6
contentArea.ScrollBarImageColor3 = Color3.fromRGB(100, 100, 100)

-- UIListLayout สำหรับการจัดเรียงเนื้อหา
local contentLayout = Instance.new("UIListLayout")
contentLayout.Parent = contentArea
contentLayout.SortOrder = Enum.SortOrder.LayoutOrder
contentLayout.Padding = UDim.new(0, 10)

-- อัปเดต CanvasSize เมื่อเนื้อหาเปลี่ยน
contentLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    contentArea.CanvasSize = UDim2.new(0, 0, 0, contentLayout.AbsoluteContentSize.Y)
end)

-- Create Tab Button
local tab1 = Instance.new("TextButton")
tab1.Size = UDim2.new(1, 0, 0, 40)
tab1.Text = "AuToFarm"
tab1.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
tab1.TextColor3 = Color3.new(1, 1, 1)
tab1.BorderSizePixel = 0
tab1.Font = Enum.Font.Gotham
tab1.TextScaled = true
tab1.Parent = sidebar

-- Create Content Page
local page1 = Instance.new("Frame")
page1.Size = UDim2.new(1, 0, 1, 0)
page1.BackgroundTransparency = 1
page1.Visible = true
page1.Name = "AuToFarm"
page1.Parent = contentArea

-- RedeemCode Button
local redeemButton = Instance.new("TextButton")
redeemButton.Parent = page1
redeemButton.Size = UDim2.new(1, -20, 0, 40)
redeemButton.Position = UDim2.new(0, 10, 0, 10)
redeemButton.Text = "RedeemCode"
redeemButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
redeemButton.TextColor3 = Color3.new(1, 1, 1)
redeemButton.BorderSizePixel = 0
redeemButton.Font = Enum.Font.Gotham
redeemButton.TextScaled = true

-- Toggle: AuToFarm
local autoFarmToggle = Instance.new("TextButton")
autoFarmToggle.Parent = page1
autoFarmToggle.Size = UDim2.new(1, -20, 0, 40)
autoFarmToggle.Position = UDim2.new(0, 10, 0, 60)
autoFarmToggle.Text = "AuToFarm: OFF"
autoFarmToggle.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
autoFarmToggle.TextColor3 = Color3.new(1, 1, 1)
autoFarmToggle.BorderSizePixel = 0
autoFarmToggle.Font = Enum.Font.Gotham
autoFarmToggle.TextScaled = true

local autoFarmStatus = false
autoFarmToggle.MouseButton1Click:Connect(function()
    autoFarmStatus = not autoFarmStatus
    autoFarmToggle.Text = "AuToFarm: " .. (autoFarmStatus and "ON" or "OFF")
end)

-- Dropdown: SeLecT STaTs
local dropdown = Instance.new("TextButton")
dropdown.Parent = page1
dropdown.Size = UDim2.new(1, -20, 0, 40)
dropdown.Position = UDim2.new(0, 10, 0, 110)
dropdown.Text = "SeLecT STaTs"
dropdown.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
dropdown.TextColor3 = Color3.new(1, 1, 1)
dropdown.BorderSizePixel = 0
dropdown.Font = Enum.Font.Gotham
dropdown.TextScaled = true

local dropdownOpen = false
local selectedOption = nil
local options = {"Melee", "Defense", "Sword", "Gun", "Fruit"}

dropdown.MouseButton1Click:Connect(function()
    dropdownOpen = not dropdownOpen
    for i, optionText in pairs(options) do
        local option = page1:FindFirstChild("Option"..i)
        if not option then
            option = Instance.new("TextButton")
            option.Name = "Option"..i
            option.Size = UDim2.new(1, -20, 0, 30)
            option.Position = UDim2.new(0, 10, 0, 110 + (i * 40))
            option.Text = optionText
            option.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
            option.TextColor3 = Color3.new(1, 1, 1)
            option.BorderSizePixel = 0
            option.Font = Enum.Font.Gotham
            option.TextScaled = true
            option.Visible = false
            option.Parent = page1

            option.MouseButton1Click:Connect(function()
                dropdown.Text = optionText
                selectedOption = optionText -- เก็บค่าที่เลือก
                dropdownOpen = false
                for j = 1, #options do
                    page1:FindFirstChild("Option"..j).Visible = false
                end
                addPoint() -- เรียกใช้งานทันทีเมื่อเลือกตัวเลือก
            end)
        end
        option.Visible = dropdownOpen
    end
end)

-- ฟังก์ชันส่งค่าไปยัง Server
local function addPoint()
    if selectedOption == "Melee" then
        local args = {
            [1] = "AddPoint",
            [2] = "Melee",
            [3] = 1
        }
        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
    elseif selectedOption == "Defense" then
        local args = {
            [1] = "AddPoint",
            [2] = "Defense",
            [3] = 1
        }
        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
    elseif selectedOption == "Sword" then
        local args = {
            [1] = "AddPoint",
            [2] = "Sword",
            [3] = 1
        }
        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
    elseif selectedOption == "Gun" then
        local args = {
            [1] = "AddPoint",
            [2] = "Gun",
            [3] = 1
        }
        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
    elseif selectedOption == "Fruit" then
        local args = {
            [1] = "AddPoint",
            [2] = "Fruit",
            [3] = 1
        }
        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
    end
end
