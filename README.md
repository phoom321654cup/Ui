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

-- UICorner for rounded edges
local corner = Instance.new("UICorner")
corner.Parent = mainFrame
corner.CornerRadius = UDim.new(0, 10)

-- Title
local title = Instance.new("TextLabel")
title.Parent = mainFrame
title.Size = UDim2.new(1, 0, 0, 50)
title.BackgroundTransparency = 1
title.Text = "Title of the library"
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

-- Tab Button
local tabButton = Instance.new("TextButton")
tabButton.Parent = sidebar
tabButton.Size = UDim2.new(1, 0, 0, 50)
tabButton.Text = "Tab 1"
tabButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
tabButton.TextColor3 = Color3.new(1, 1, 1)
tabButton.BorderSizePixel = 0
tabButton.Font = Enum.Font.Gotham
tabButton.TextScaled = true

-- Content Area
local contentArea = Instance.new("Frame")
contentArea.Parent = mainFrame
contentArea.Size = UDim2.new(1, -100, 1, -50)
contentArea.Position = UDim2.new(0, 100, 0, 50)
contentArea.BackgroundTransparency = 1

-- Button
local button = Instance.new("TextButton")
button.Parent = contentArea
button.Size = UDim2.new(1, -20, 0, 50)
button.Position = UDim2.new(0, 10, 0, 10)
button.Text = "Button!"
button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
button.TextColor3 = Color3.new(1, 1, 1)
button.BorderSizePixel = 0
button.Font = Enum.Font.Gotham
button.TextScaled = true
button.MouseButton1Click:Connect(function()
    print("Button clicked!")
end)

-- Toggle
local toggle = Instance.new("TextButton")
toggle.Parent = contentArea
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
    print("Toggle state:", isToggled)
end)

-- Color Picker (ตัวอย่างง่าย ๆ )
local colorPicker = Instance.new("TextButton")
colorPicker.Parent = contentArea
colorPicker.Size = UDim2.new(0, 50, 0, 50)
colorPicker.Position = UDim2.new(0, 10, 0, 130)
colorPicker.BackgroundColor3 = Color3.new(1, 0, 0)
colorPicker.BorderSizePixel = 0
colorPicker.Text = ""
colorPicker.MouseButton1Click:Connect(function()
    colorPicker.BackgroundColor3 = Color3.new(math.random(), math.random(), math.random())
end)

-- Slider (ทำแบบง่าย ๆ ด้วย TextBox)
local slider = Instance.new("TextBox")
slider.Parent = contentArea
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
        print("Slider value:", slider.Text)
    end
end)
