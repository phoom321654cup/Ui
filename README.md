local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui")
gui.Parent = player:WaitForChild("PlayerGui")
gui.Name = "CustomUI"
gui.ResetOnSpawn = false

-- Main Frame
local mainFrame = Instance.new("Frame")
mainFrame.Parent = gui
mainFrame.Size = UDim2.new(0, 400, 0, 450)
mainFrame.Position = UDim2.new(0.5, -200, 0.5, -225)
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

-- Slider for STaTs (min = 1, max = 99)
local sliderFrame = Instance.new("Frame")
sliderFrame.Parent = page1
sliderFrame.Size = UDim2.new(1, -20, 0, 50)
sliderFrame.Position = UDim2.new(0, 10, 0, 10)
sliderFrame.BackgroundTransparency = 1

local sliderLabel = Instance.new("TextLabel")
sliderLabel.Parent = sliderFrame
sliderLabel.Size = UDim2.new(1, 0, 0.4, 0)
sliderLabel.Position = UDim2.new(0, 0, 0, 0)
sliderLabel.BackgroundTransparency = 1
sliderLabel.Text = "SeLecT STaTs: 1"
sliderLabel.TextColor3 = Color3.new(1, 1, 1)
sliderLabel.TextScaled = true
sliderLabel.Font = Enum.Font.Gotham

local slider = Instance.new("TextButton")
slider.Parent = sliderFrame
slider.Size = UDim2.new(1, 0, 0.6, 0)
slider.Position = UDim2.new(0, 0, 0.4, 0)
slider.Text = ""
slider.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
slider.BorderSizePixel = 0
slider.AutoButtonColor = false

local sliderKnob = Instance.new("Frame")
sliderKnob.Parent = slider
sliderKnob.Size = UDim2.new(0, 10, 1, 0)
sliderKnob.Position = UDim2.new(0, 0, 0, 0)
sliderKnob.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
sliderKnob.BorderSizePixel = 0

local draggingSlider = false

slider.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        draggingSlider = true
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        draggingSlider = false
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if draggingSlider and input.UserInputType == Enum.UserInputType.MouseMovement then
        local sliderPos = math.clamp(input.Position.X - slider.AbsolutePosition.X, 0, slider.AbsoluteSize.X)
        local percentage = sliderPos / slider.AbsoluteSize.X
        local value = math.floor(1 + (98 * percentage))
        sliderKnob.Position = UDim2.new(percentage, 0, 0, 0)
        sliderLabel.Text = "SeLecT STaTs: " .. value
    end
end)

-- Toggle: STarT Up STaTs
local startUpToggle = Instance.new("TextButton")
startUpToggle.Parent = page1
startUpToggle.Size = UDim2.new(1, -20, 0, 40)
startUpToggle.Position = UDim2.new(0, 10, 0, 70)
startUpToggle.Text = "STarT Up STaTs: OFF"
startUpToggle.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
startUpToggle.TextColor3 = Color3.new(1, 1, 1)
startUpToggle.BorderSizePixel = 0
startUpToggle.Font = Enum.Font.Gotham
startUpToggle.TextScaled = true

local startUpStatus = false
startUpToggle.MouseButton1Click:Connect(function()
    startUpStatus = not startUpStatus
    startUpToggle.Text = "STarT Up STaTs: " .. (startUpStatus and "ON" or "OFF")
end)
