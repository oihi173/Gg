local code = [[
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local UserInputService = game:GetService("UserInputService")
local StarterGui = game:GetService("StarterGui")

local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

-- Criar animação
local animation = Instance.new("Animation")
animation.AnimationId = "rbxassetid://2623795"
local animationTrack = humanoid:LoadAnimation(animation)

local animPlaying = false

-- Criar GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "AnimationToggleGui"
screenGui.ResetOnSpawn = false
screenGui.Parent = player:WaitForChild("PlayerGui")

local button = Instance.new("TextButton")
button.Size = UDim2.new(0, 150, 0, 50)
button.Position = UDim2.new(0.5, -75, 0.8, 0)
button.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
button.TextColor3 = Color3.new(1,1,1)
button.Text = "Iniciar animação"
button.Font = Enum.Font.SourceSansBold
button.TextScaled = true
button.Parent = screenGui

-- Função para alternar animação e texto do botão
local function toggleAnimation()
    if animPlaying then
        animationTrack:Stop()
        animPlaying = false
        button.Text = "Iniciar animação"
    else
        animationTrack:Play()
        animPlaying = true
        button.Text = "Parar animação"
    end
end

button.MouseButton1Click:Connect(toggleAnimation)

-- Atualizar humanoid caso o personagem renasça
player.CharacterAdded:Connect(function(char)
    character = char
    humanoid = character:WaitForChild("Humanoid")
    animationTrack = humanoid:LoadAnimation(animation)
    animPlaying = false
    button.Text = "Iniciar animação"
end)
]]

loadstring(code)()
