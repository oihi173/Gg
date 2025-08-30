-- StarterGui > LocalScript
local player = game.Players.LocalPlayer

local gui = Instance.new("ScreenGui")
gui.Name = "PainelCustom"
gui.ResetOnSpawn = false
gui.Parent = player:WaitForChild("PlayerGui")

-- Botão Abrir (⚪)
local abrirBtn = Instance.new("TextButton")
abrirBtn.Size = UDim2.new(0, 40, 0, 40)
abrirBtn.Position = UDim2.new(0, 10, 0, 200)
abrirBtn.Text = "O"
abrirBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 255)
abrirBtn.TextScaled = true
abrirBtn.Parent = gui

-- Painel
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 250, 0, 200)
frame.Position = UDim2.new(0.3, 0, 0.3, 0)
frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
frame.Active = true
frame.Visible = false
frame.Parent = gui

-- Botão Fechar (❌)
local fecharBtn = Instance.new("TextButton")
fecharBtn.Size = UDim2.new(0, 30, 0, 30)
fecharBtn.Position = UDim2.new(1, -35, 0, 5)
fecharBtn.Text = "X"
fecharBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
fecharBtn.TextScaled = true
fecharBtn.Parent = frame

-- Abrir/Fechar
abrirBtn.MouseButton1Click:Connect(function()
	frame.Visible = true
	abrirBtn.Visible = false
end)

fecharBtn.MouseButton1Click:Connect(function()
	frame.Visible = false
	abrirBtn.Visible = true
end)

-- 🔹 Sistema de arrastar (substitui Draggable)
local UserInputService = game:GetService("UserInputService")

local dragging, dragInput, dragStart, startPos

local function update(input)
	local delta = input.Position - dragStart
	frame.Position = UDim2.new(
		startPos.X.Scale,
		startPos.X.Offset + delta.X,
		startPos.Y.Scale,
		startPos.Y.Offset + delta.Y
	)
end

frame.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = true
		dragStart = input.Position
		startPos = frame.Position

		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
			end
		end)
	end
end)

frame.InputChanged:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseMovement then
		dragInput = input
	end
end)

UserInputService.InputChanged:Connect(function(input)
	if input == dragInput and dragging then
		update(input)
	end
end)
