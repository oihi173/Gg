local UIS = game:GetService("UserInputService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- GUI principal
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "PainelMusica"
screenGui.Parent = player:WaitForChild("PlayerGui")

-- Painel Principal
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 300, 0, 400)
mainFrame.Position = UDim2.new(0.3, 0, 0.2, 0)
mainFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
mainFrame.Active = true
mainFrame.Visible = false -- Começa fechado
mainFrame.Parent = screenGui

-- Botão Abrir/Fechar
local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(0, 100, 0, 30)
toggleButton.Position = UDim2.new(0, 10, 0, 10)
toggleButton.Text = "Abrir Painel"
toggleButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
toggleButton.Parent = screenGui

-- Música
local sound = Instance.new("Sound")
sound.Name = "Musica"
sound.Parent = screenGui

-- Input de ID da música
local musicInput = Instance.new("TextBox")
musicInput.Size = UDim2.new(0, 260, 0, 30)
musicInput.Position = UDim2.new(0, 20, 0, 20)
musicInput.PlaceholderText = "Coloque o ID da música aqui"
musicInput.Text = ""
musicInput.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
musicInput.Parent = mainFrame

-- Botão de tocar música
local playButton = Instance.new("TextButton")
playButton.Size = UDim2.new(0, 100, 0, 30)
playButton.Position = UDim2.new(0, 20, 0, 60)
playButton.Text = "Tocar Música"
playButton.BackgroundColor3 = Color3.fromRGB(60, 100, 60)
playButton.Parent = mainFrame

-- Opções extras
local options = {}
local numOpcoes = 3
for i = 1, numOpcoes do
	local optionBtn = Instance.new("TextButton")
	optionBtn.Size = UDim2.new(0, 240, 0, 30)
	optionBtn.Position = UDim2.new(0, 30, 0, 110 + (i - 1) * 40)
	optionBtn.Text = "Opção " .. i
	optionBtn.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
	optionBtn.Parent = mainFrame

	-- Emblema de desativado (X vermelho)
	local emblema = Instance.new("ImageLabel")
	emblema.Size = UDim2.new(0, 24, 0, 24)
	emblema.Position = UDim2.new(1, -30, 0.5, -12)
	emblema.BackgroundTransparency = 1
	emblema.Image = "rbxassetid://6031091002" -- X vermelho
	emblema.Visible = false
	emblema.Parent = optionBtn

	options[i] = {
		button = optionBtn,
		emblema = emblema,
		ativo = true
	}

	-- Alternar opção
	optionBtn.MouseButton1Click:Connect(function()
		local opt = options[i]
		opt.ativo = not opt.ativo
		opt.emblema.Visible = not opt.ativo
	end)
end

-- Abrir/Fechar painel
local isOpen = false
toggleButton.MouseButton1Click:Connect(function()
	isOpen = not isOpen
	mainFrame.Visible = isOpen
	toggleButton.Text = isOpen and "Fechar Painel" or "Abrir Painel"
end)

-- Tocar música ao clicar
playButton.MouseButton1Click:Connect(function()
	local id = musicInput.Text
	if id ~= "" then
		sound:Stop()
		sound.SoundId = "rbxassetid://" .. id
		sound:Play()
	end
end)
