local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui")
gui.Parent = player:WaitForChild("PlayerGui")
gui.Name = "CustomUI"
gui.ResetOnSpawn = false

-- Main Frame
local mainFrame = Instance.new("Frame")
mainFrame.Parent = gui
mainFrame.Size = UDim2.new(0, 400, 0, 350)
mainFrame.Position = UDim2.new(0.5, -200, 0.5, -175)
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
local options = {"Option 1", "Option 2", "Option 3"}

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
                dropdownOpen = false
                for j = 1, #options do
                    page1:FindFirstChild("Option"..j).Visible = false
                end
            end)
        end
        option.Visible = dropdownOpen
    end
end)

-- Refresh Dropdown Button
local refreshButton = Instance.new("TextButton")
refreshButton.Parent = page1
refreshButton.Size = UDim2.new(1, -20, 0, 40)
refreshButton.Position = UDim2.new(0, 10, 0, 160)
refreshButton.Text = "Refresh Dropdown"
refreshButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
refreshButton.TextColor3 = Color3.new(1, 1, 1)
refreshButton.BorderSizePixel = 0
refreshButton.Font = Enum.Font.Gotham
refreshButton.TextScaled = true

-- ฟังก์ชันสำหรับเคลียร์ Dropdown
local function ClearDropdown()
    for i = 1, #options do
        local oldOption = page1:FindFirstChild("Option"..i)
        if oldOption then
            oldOption:Destroy()
        end
    end

    -- รีเซ็ตตัวแปร options และ dropdownOpen
    options = {}
    dropdownOpen = false

    -- ตั้งค่า Text ให้กลับเป็นค่าเริ่มต้น
    dropdown.Text = "SeLecT STaTs"
end

-- เมื่อกดปุ่ม Refresh จะลบตัวเลือกทั้งหมดใน Dropdown
refreshButton.MouseButton1Click:Connect(function()
    ClearDropdown()
end)
