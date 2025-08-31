-- Criar a interface
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "TeleportPanel"
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false

-- Função de teleporte
local function teleportTo(position)
    local character = game.Players.LocalPlayer.Character
    if character and character:FindFirstChild("HumanoidRootPart") then
        character:MoveTo(position)
    end
end

-- Dados dos botões e posições
local teleportPoints = {
    {name = "Condenada 1", position = Vector3.new(-190.56, 3.86, 1424.11)},
    {name = "Condenada 2", position = Vector3.new(-206.71, 5, 2562.09)}, -- Valor Y assumido
    {name = "Condenada 3", position = Vector3.new(197.24, 35.66, 3309.48)},
    {name = "Condenada 4", position = Vector3.new(56.11, -32557, 9481.89)},
}

-- Criar painel e botões
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 200, 0, 250)
frame.Position = UDim2.new(0, 10, 0.5, -125)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BorderSizePixel = 0
frame.Parent = ScreenGui

-- Layout dos botões
local layout = Instance.new("UIListLayout")
layout.Parent = frame
layout.Padding = UDim.new(0, 10)

-- Criar botões
for _, data in ipairs(teleportPoints) do
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, -20, 0, 40)
    btn.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.Font = Enum.Font.SourceSansBold
    btn.TextSize = 18
    btn.Text = data.name
    btn.Parent = frame

    btn.MouseButton1Click:Connect(function()
        teleportTo(data.position)
    end)
end
