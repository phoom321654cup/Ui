-- สร้าง ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = game.Players.LocalPlayer.PlayerGui

-- สร้าง Frame หลัก
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 300, 0, 500)  -- ขนาดของหน้าต่าง UI
mainFrame.Position = UDim2.new(0.5, -150, 0.5, -250)  -- ตำแหน่งกลางหน้าจอ
mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
mainFrame.Parent = screenGui

-- สร้าง UIListLayout เพื่อจัดเรียงเนื้อหาใน frame
local listLayout = Instance.new("UIListLayout")
listLayout.Parent = mainFrame
listLayout.FillDirection = Enum.FillDirection.Vertical
listLayout.SortOrder = Enum.SortOrder.LayoutOrder

-- สร้างปุ่ม Main
local mainButton = Instance.new("TextButton")
mainButton.Size = UDim2.new(1, 0, 0, 40)
mainButton.Text = "Main"
mainButton.BackgroundColor3 = Color3.fromRGB(100, 100, 255)
mainButton.TextColor3 = Color3.fromRGB(255, 255, 255)
mainButton.Parent = mainFrame

-- สร้างปุ่ม Combat
local combatButton = Instance.new("TextButton")
combatButton.Size = UDim2.new(1, 0, 0, 40)
combatButton.Text = "Combat"
combatButton.BackgroundColor3 = Color3.fromRGB(100, 255, 100)
combatButton.TextColor3 = Color3.fromRGB(255, 255, 255)
combatButton.Parent = mainFrame

-- สร้างปุ่ม Visc
local viscButton = Instance.new("TextButton")
viscButton.Size = UDim2.new(1, 0, 0, 40)
viscButton.Text = "Visc"
viscButton.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
viscButton.TextColor3 = Color3.fromRGB(255, 255, 255)
viscButton.Parent = mainFrame

-- สร้าง Toggle Button (เปิด-ปิด)
local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(1, 0, 0, 40)
toggleButton.Text = "Toggle"
toggleButton.BackgroundColor3 = Color3.fromRGB(150, 150, 150)
toggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleButton.Parent = mainFrame

toggleButton.MouseButton1Click:Connect(function()
    if toggleButton.Text == "Toggle" then
        toggleButton.Text = "Toggled"
    else
        toggleButton.Text = "Toggle"
    end
end)

-- สร้าง Dropdown
local dropdownButton = Instance.new("TextButton")
dropdownButton.Size = UDim2.new(1, 0, 0, 40)
dropdownButton.Text = "Dropdown"
dropdownButton.BackgroundColor3 = Color3.fromRGB(200, 200, 100)
dropdownButton.TextColor3 = Color3.fromRGB(255, 255, 255)
dropdownButton.Parent = mainFrame

local dropdownFrame = Instance.new("Frame")
dropdownFrame.Size = UDim2.new(1, 0, 0, 100)
dropdownFrame.Position = UDim2.new(0, 0, 0, 40)
dropdownFrame.BackgroundColor3 = Color3.fromRGB(200, 200, 200)
dropdownFrame.Visible = false
dropdownFrame.Parent = mainFrame

local dropdownOption1 = Instance.new("TextButton")
dropdownOption1.Size = UDim2.new(1, 0, 0, 30)
dropdownOption1.Text = "Option 1"
dropdownOption1.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
dropdownOption1.TextColor3 = Color3.fromRGB(0, 0, 0)
dropdownOption1.Parent = dropdownFrame

local dropdownOption2 = Instance.new("TextButton")
dropdownOption2.Size = UDim2.new(1, 0, 0, 30)
dropdownOption2.Text = "Option 2"
dropdownOption2.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
dropdownOption2.TextColor3 = Color3.fromRGB(0, 0, 0)
dropdownOption2.Parent = dropdownFrame

dropdownButton.MouseButton1Click:Connect(function()
    dropdownFrame.Visible = not dropdownFrame.Visible
end)

-- สร้าง Slider
local sliderFrame = Instance.new("Frame")
sliderFrame.Size = UDim2.new(1, 0, 0, 40)
sliderFrame.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
sliderFrame.Parent = mainFrame

local slider = Instance.new("TextButton")
slider.Size = UDim2.new(1, -20, 0, 10)
slider.Position = UDim2.new(0, 10, 0, 15)
slider.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
slider.Text = ""
slider.Parent = sliderFrame

local sliderIndicator = Instance.new("Frame")
sliderIndicator.Size = UDim2.new(0.5, 0, 1, 0)
sliderIndicator.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
sliderIndicator.Parent = slider

slider.MouseButton1Down:Connect(function()
    local mouse = game.Players.LocalPlayer:GetMouse()
    local startPos = slider.Position.X.Offset
    local startMouse = mouse.X

    local function updateSlider()
        local offset = mouse.X - startMouse + startPos
        offset = math.clamp(offset, 0, slider.Size.X.Offset - sliderIndicator.Size.X.Offset)
        sliderIndicator.Position = UDim2.new(0, offset, 0, 0)
    end

    game:GetService("UserInputService").InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement then
            updateSlider()
        end
    end)
end)

-- สร้าง Scrollbar
local scrollBar = Instance.new("ScrollingFrame")
scrollBar.Size = UDim2.new(0, 20, 1, 0)
scrollBar.Position = UDim2.new(1, 0, 0, 0)
scrollBar.BackgroundColor3 = Color3.fromRGB(150, 150, 150)
scrollBar.ScrollBarThickness = 10
scrollBar.Parent = mainFrame
