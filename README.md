local screenGui = Instance.new("ScreenGui")
screenGui.Name = "PainelLinks"
screenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

-- Criando o Frame principal
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 300, 0, 200)
frame.Position = UDim2.new(0.3, 0, 0.3, 0)
frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
frame.Active = true
frame.Draggable = true -- Permite mover o painel
frame.Visible = false
frame.Parent = screenGui

-- Botão para abrir/fechar o painel
local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(0, 120, 0, 40)
toggleButton.Position = UDim2.new(0, 10, 0, 10)
toggleButton.Text = "Abrir Painel"
toggleButton.Parent = screenGui

-- Função de abrir/fechar
toggleButton.MouseButton1Click:Connect(function()
	frame.Visible = not frame.Visible
	if frame.Visible then
		toggleButton.Text = "Fechar Painel"
	else
		toggleButton.Text = "Abrir Painel"
	end
end)

-- Criando exemplo de botão com link
local linkButton = Instance.new("TextButton")
linkButton.Size = UDim2.new(0, 200, 0, 40)
linkButton.Position = UDim2.new(0, 50, 0, 80)
linkButton.Text = "Abrir Site Roblox"
linkButton.BackgroundColor3 = Color3.fromRGB(80, 80, 200)
linkButton.Parent = frame

-- Quando clicar, abre o navegador
linkButton.MouseButton1Click:Connect(function()
	game:GetService("StarterGui"):SetCore("OpenBrowserWindow", "https://www.roblox.com")
end)
