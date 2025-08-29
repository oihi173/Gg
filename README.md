-- Coloque este script dentro de um ScreenGui no StarterGui

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local camera = workspace.CurrentCamera

local UserInputService = game:GetService("UserInputService")

-- Criar botão
local screenGui = script.Parent
local button = Instance.new("TextButton")
button.Size = UDim2.new(0, 150, 0, 50)
button.Position = UDim2.new(0.05, 0, 0.8, 0)
button.Text = "Ativar Modo Reto"
button.BackgroundColor3 = Color3.fromRGB(50, 150, 250)
button.TextColor3 = Color3.fromRGB(255,255,255)
button.Parent = screenGui

-- Variável para ativar/desativar
local ativo = false

-- Função que mantém o personagem reto
game:GetService("RunService").RenderStepped:Connect(function()
	if ativo and character and character:FindFirstChild("HumanoidRootPart") then
		local lookVector = camera.CFrame.LookVector
		local newCFrame = CFrame.new(character.HumanoidRootPart.Position, character.HumanoidRootPart.Position + Vector3.new(lookVector.X, 0, lookVector.Z))
		character.HumanoidRootPart.CFrame = CFrame.new(character.HumanoidRootPart.Position) * CFrame.Angles(0, math.atan2(-lookVector.X, -lookVector.Z), 0)
	end
end)

-- Botão para ativar/desativar
button.MouseButton1Click:Connect(function()
	ativo = not ativo
	if ativo then
		button.Text = "Desativar Modo Reto"
		button.BackgroundColor3 = Color3.fromRGB(250, 100, 100)
	else
		button.Text = "Ativar Modo Reto"
		button.BackgroundColor3 = Color3.fromRGB(50, 150, 250)
	end
end)

-- Garante que funcione mesmo se o player morrer e renascer
player.CharacterAdded:Connect(function(char)
	character = char
	humanoid = char:WaitForChild("Humanoid")
end)
