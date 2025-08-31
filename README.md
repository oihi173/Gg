loadstring([[
-- Painel com botão para ativar/desativar a hitbox com som de clique

local Players = game:GetService("Players")
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

-- Função para remover a hitbox
local function removeHitbox()
    for _, part in pairs(character:GetDescendants()) do
        if part:IsA("BasePart") and part.CanCollide then
            part.Size = Vector3.new(0.1, 0.1, 0.1)
            part.Transparency = 1
        end
    end
end

-- Função para restaurar a hitbox
local function restoreHitbox()
    for _, part in pairs(character:GetDescendants()) do
        if part:IsA("BasePart") then
            part.Size = Vector3.new(2, 2, 1)
            part.Transparency = 0
        end
    end
end

-- Criar GUI
local ScreenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
ScreenGui.Name = "HitboxToggleGUI"

local ToggleButton = Instance.new("TextButton", ScreenGui)
ToggleButton.Size = UDim2.new(0, 200, 0, 50)
ToggleButton.Position = UDim2.new(0.5, -100, 0.9, 0)
ToggleButton.Text = "Desativar Hitbox"
ToggleButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
ToggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleButton.Font = Enum.Font.SourceSansBold
ToggleButton.TextSize = 20

-- Adicionar som ao botão
local clickSound = Instance.new("Sound", ToggleButton)
clickSound.SoundId = "rbxassetid://12221967" -- Som de clique padrão da Roblox
clickSound.Volume = 1
clickSound.Name = "ClickSound"

local hitboxDisabled = false

ToggleButton.MouseButton1Click:Connect(function()
    clickSound:Play()
    hitboxDisabled = not hitboxDisabled
    if hitboxDisabled then
        removeHitbox()
        ToggleButton.Text = "Ativar Hitbox"
    else
        restoreHitbox()
        ToggleButton.Text = "Desativar Hitbox"
    end
end)

-- Atualiza se o personagem renascer
player.CharacterAdded:Connect(function(char)
    character = char
    if hitboxDisabled then
        wait(1)
        removeHitbox()
    end
end)
]])()
