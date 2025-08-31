-- LocalScript dentro de MusicGui

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Criar GUI
local screenGui = script.Parent

-- Lista de músicas
local musicNames = {"Musica1", "Musica2", "Musica3"} -- nomes dos objetos Sound no ReplicatedStorage
local currentMusicIndex = 1
local currentSound = nil

-- Criar Frame
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0.5, 0, 0.4, 0)
frame.Position = UDim2.new(0.25, 0, 0.55, 0)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BorderSizePixel = 0
frame.Parent = screenGui

-- Criar título
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0.2, 0)
title.Position = UDim2.new(0, 0, 0, 0)
title.BackgroundTransparency = 1
title.Text = "🎵 Painel de Música"
title.TextScaled = true
title.TextColor3 = Color3.new(1, 1, 1)
title.Font = Enum.Font.SourceSansBold
title.Parent = frame

-- Função para criar botão
local function createButton(text, position, callback)
	local button = Instance.new("TextButton")
	button.Size = UDim2.new(0.3, 0, 0.25, 0)
	button.Position = position
	button.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
	button.TextColor3 = Color3.new(1, 1, 1)
	button.Font = Enum.Font.SourceSansBold
	button.TextScaled = true
	button.Text = text
	button.Parent = frame
	button.MouseButton1Click:Connect(callback)
end

-- Tocar música atual
local function playMusic()
	-- Parar música atual se tiver
	if currentSound then
		currentSound:Stop()
		currentSound:Destroy()
	end

	local soundName = musicNames[currentMusicIndex]
	local soundTemplate = ReplicatedStorage:FindFirstChild(soundName)
	if soundTemplate and soundTemplate:IsA("Sound") then
		currentSound = soundTemplate:Clone()
		currentSound.Parent = workspace
		currentSound:Play()
	end
end

-- Criar botões
createButton("▶️ Tocar", UDim2.new(0.05, 0, 0.3, 0), function()
	playMusic()
end)

createButton("⏸️ Pausar", UDim2.new(0.35, 0, 0.3, 0), function()
	if currentSound and currentSound.IsPlaying then
		currentSound:Pause()
	end
end)

createButton("⏭️ Próxima", UDim2.new(0.65, 0, 0.3, 0), function()
	currentMusicIndex += 1
	if currentMusicIndex > #musicNames then
		currentMusicIndex = 1
	end
	playMusic()
end)
