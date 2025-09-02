-- LocalScript dentro de StarterPlayerScripts
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local player = Players.LocalPlayer

local flyEnabled = false
local bv, bg
local speed = 60
local moveDir = Vector3.new(0,0,0)

-- Criar GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "AdminGUI"
screenGui.ResetOnSpawn = false
screenGui.Parent = player:WaitForChild("PlayerGui")

-- Painel
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 300, 0, 400)
frame.Position = UDim2.new(0.5, -150, 0.5, -200)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.Active = true
frame.Draggable = true
frame.Visible = false
frame.Parent = screenGui

-- Título
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 50)
title.Text = "😎 Painel ADM"
title.TextScaled = true
title.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
title.TextColor3 = Color3.new(1, 1, 1)
title.Font = Enum.Font.FredokaOne
title.Parent = frame

-- Botão abrir/fechar painel
local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(0, 120, 0, 40)
toggleButton.Position = UDim2.new(0, 10, 0, 10)
toggleButton.Text = "Abrir Painel"
toggleButton.TextScaled = true
toggleButton.BackgroundColor3 = Color3.fromRGB(100, 100, 255)
toggleButton.TextColor3 = Color3.new(1, 1, 1)
toggleButton.Font = Enum.Font.GothamBold
toggleButton.Parent = screenGui

toggleButton.MouseButton1Click:Connect(function()
	frame.Visible = not frame.Visible
	if frame.Visible then
		toggleButton.Text = "Fechar Painel"
	else
		toggleButton.Text = "Abrir Painel"
	end
end)

-- Botão Fly
local flyButton = Instance.new("TextButton")
flyButton.Size = UDim2.new(0.8, 0, 0, 50)
flyButton.Position = UDim2.new(0.1, 0, 0.2, 0)
flyButton.Text = "Fly OFF"
flyButton.TextScaled = true
flyButton.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
flyButton.TextColor3 = Color3.new(1, 1, 1)
flyButton.Font = Enum.Font.GothamBold
flyButton.Parent = frame

-- Botão Subir
local upButton = Instance.new("TextButton")
upButton.Size = UDim2.new(0.35, 0, 0, 50)
upButton.Position = UDim2.new(0.1, 0, 0.4, 0)
upButton.Text = "↑ Subir"
upButton.TextScaled = true
upButton.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
upButton.TextColor3 = Color3.new(1, 1, 1)
upButton.Font = Enum.Font.GothamBold
upButton.Parent = frame

-- Botão Descer
local downButton = Instance.new("TextButton")
downButton.Size = UDim2.new(0.35, 0, 0, 50)
downButton.Position = UDim2.new(0.55, 0, 0.4, 0)
downButton.Text = "↓ Descer"
downButton.TextScaled = true
downButton.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
downButton.TextColor3 = Color3.new(1, 1, 1)
downButton.Font = Enum.Font.GothamBold
downButton.Parent = frame

-- Criar controladores de voo
local function createControllers(hrp)
	bv = Instance.new("BodyVelocity")
	bv.MaxForce = Vector3.new(1e5,1e5,1e5)
	bv.P = 1250
	bv.Parent = hrp

	bg = Instance.new("BodyGyro")
	bg.MaxTorque = Vector3.new(1e5,1e5,1e5)
	bg.P = 3000
	bg.Parent = hrp
end

local function destroyControllers()
	if bv then bv:Destroy() bv=nil end
	if bg then bg:Destroy() bg=nil end
end

local function toggleFly()
	flyEnabled = not flyEnabled
	if flyEnabled then
		flyButton.Text = "Fly ON"
	else
		flyButton.Text = "Fly OFF"
		moveDir = Vector3.new(0,0,0)
		destroyControllers()
	end
end

flyButton.MouseButton1Click:Connect(toggleFly)

-- Botões de subir/descer
upButton.MouseButton1Down:Connect(function()
	if flyEnabled then
		moveDir = Vector3.new(0,1,0)
	end
end)
upButton.MouseButton1Up:Connect(function()
	if flyEnabled then
		moveDir = Vector3.new(0,0,0)
	end
end)

downButton.MouseButton1Down:Connect(function()
	if flyEnabled then
		moveDir = Vector3.new(0,-1,0)
	end
end)
downButton.MouseButton1Up:Connect(function()
	if flyEnabled then
		moveDir = Vector3.new(0,0,0)
	end
end)

-- Loop do voo
RunService.RenderStepped:Connect(function()
	local char = player.Character
	if not char then return end
	local hrp = char:FindFirstChild("HumanoidRootPart")
	local cam = workspace.CurrentCamera

	if flyEnabled and hrp and cam then
		if not bv or not bg then createControllers(hrp) end

		bv.Velocity = moveDir * speed
		bg.CFrame = cam.CFrame
	end
end)
