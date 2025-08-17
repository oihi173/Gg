local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local ToggleButton = Instance.new("TextButton")
local FlyButton = Instance.new("TextButton")
local ESPButton = Instance.new("TextButton")
local TPButton = Instance.new("TextButton")

ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.Name = "CustomPanel"

MainFrame.Size = UDim2.new(0, 200, 0, 250)
MainFrame.Position = UDim2.new(0, 50, 0, 50)
MainFrame.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
MainFrame.Visible = true
MainFrame.Parent = ScreenGui
MainFrame.Active = true
MainFrame.Draggable = true

ToggleButton.Size = UDim2.new(0, 200, 0, 30)
ToggleButton.Position = UDim2.new(0, 0, 0, 0)
ToggleButton.Text = "Fechar Painel"
ToggleButton.Parent = MainFrame

FlyButton.Size = UDim2.new(0, 200, 0, 30)
FlyButton.Position = UDim2.new(0, 0, 0, 40)
FlyButton.Text = "Ativar Fly"
FlyButton.Parent = MainFrame

ESPButton.Size = UDim2.new(0, 200, 0, 30)
ESPButton.Position = UDim2.new(0, 0, 0, 80)
ESPButton.Text = "Ativar ESP"
ESPButton.Parent = MainFrame

TPButton.Size = UDim2.new(0, 200, 0, 30)
TPButton.Position = UDim2.new(0, 0, 0, 120)
TPButton.Text = "Ir para Landos"
TPButton.Parent = MainFrame

-- Toggle GUI
ToggleButton.MouseButton1Click:Connect(function()
	MainFrame.Visible = not MainFrame.Visible
end)

-- Fly Function
local flying = false
FlyButton.MouseButton1Click:Connect(function()
	flying = not flying
	local player = game.Players.LocalPlayer
	local char = player.Character or player.CharacterAdded:Wait()
	local humanoidRootPart = char:WaitForChild("HumanoidRootPart")

	local bodyGyro = Instance.new("BodyGyro", humanoidRootPart)
	local bodyVelocity = Instance.new("BodyVelocity", humanoidRootPart)
	bodyGyro.P = 9e4
	bodyGyro.maxTorque = Vector3.new(9e9, 9e9, 9e9)
	bodyGyro.cframe = humanoidRootPart.CFrame
	bodyVelocity.velocity = Vector3.new(0, 0, 0)
	bodyVelocity.maxForce = Vector3.new(9e9, 9e9, 9e9)

	while flying do
		bodyVelocity.velocity = Vector3.new(0, 50, 0)
		wait()
	end

	bodyGyro:Destroy()
	bodyVelocity:Destroy()
end)

-- ESP Function
ESPButton.MouseButton1Click:Connect(function()
	for _, player in pairs(game.Players:GetPlayers()) do
		if player ~= game.Players.LocalPlayer and player.Character then
			local highlight = Instance.new("Highlight")
			highlight.Adornee = player.Character
			highlight.FillColor = Color3.new(1, 0, 0)
			highlight.OutlineColor = Color3.new(1, 1, 1)
			highlight.Parent = player.Character
		end
	end
end)

-- Teleport to Landos
TPButton.MouseButton1Click:Connect(function()
	local player = game.Players.LocalPlayer
	local char = player.Character or player.CharacterAdded:Wait()
	local hrp = char:WaitForChild("HumanoidRootPart")

	local landos = workspace:FindFirstChild("Landos")
	if landos then
		hrp.CFrame = landos.CFrame + Vector3.new(0, 5, 0)
	else
		warn("Landos não encontrado no workspace.")
	end
end)
