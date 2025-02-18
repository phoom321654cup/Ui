local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/jensonhirst/Orion/main/source')))()
local Window = OrionLib:MakeWindow({Name = "POOM HUB", HidePremium = false, SaveConfig = true, ConfigFolder = "seuk"})
local Tab1 = Window:MakeTab({
	Name = "Key before use script",
	Icon = "rbxassetid://4483345998",
	PremiumOnly = false
})

function incorrectkey()
OrionLib:MakeNotification({
	Name = "คีย์ผิดอีควายอ้อหน่อยสุชาดานิภาพร",
	Content = "ไปหาคีย์ใหม่มาอีหน่อยอ้อสุนิ",
	Image = "rbxassetid://4483345998",
	Time = 10
})
end

function correctkey()
    player:Kick("ควายอ้อโดนเตะออกไปไปไอสัส")
end

_G.truekey = "EorEnoyEsuEni"
_G.input = "string"

local Section = Tab1:AddSection({
	Name = "Key"
})
Tab1:AddTextbox({
	Name = "Paste Key",
	Default = "ใส่คีย์สิอีอ้อ",
	TextDisappear = true,
	Callback = function(key)
	_G.input = key
	end	  
})
Tab1:AddButton({
	Name = "Check Key",
	Callback = function()
	if _G.input == _G.truekey then
	correctkey()
	else incorrectkey()
	end    
})
