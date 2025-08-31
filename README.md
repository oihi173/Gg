-- Roblox Lua Script: Painel de Teleporte Gui

local player = game.Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")
local TweenService = game:GetService("TweenService")

-- Painel principal
local panel = Instance.new("Frame")
panel.Name = "TeleportPanel"
panel.Size = UDim2.new(0, 220, 0, 220)
panel.Position = UDim2.new(0, 20, 0, 100)
panel.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
panel.BackgroundTransparency = 0.1
panel.BorderSizePixel = 0
panel.Active = true
panel.Draggable = true
panel.Parent = PlayerGui

-- Cantos arredondados
local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 12)
corner.Parent = panel

-- Layout para organização dos botões
local layout = Instance.new("UIListLayout")
layout.Padding = UDim.new(0, 10)
layout.FillDirection = Enum.FillDirection.Vertical
layout.HorizontalAlignment = Enum.HorizontalAlignment.Center
layout.VerticalAlignment = Enum.VerticalAlignment.Top
layout.Parent = panel

-- Posições dos teleportes
local teleportPositions = {
    ["Gui 1"] = {
        position = Vector3.new(178.24, 3, -382.88),
        icon = "rbxassetid://6034996695" -- mapa
    },
    ["Gui 2"] = {
        position = Vector3.new(-191.88, 33.95, -290.25),
        icon = "rbxassetid://6035007858" -- ponte
    },
    ["Gui 3"] = {
        position = Vector3.new(-407.48, 65.96, -444.19),
        icon = "rbxassetid://6034982914" -- montanha
    }
}

-- Som de clique (opcional)
local clickSound = Instance.new("Sound")
clickSound.SoundId = "rbxassetid://12222242"
clickSound.Volume = 0.5
clickSound.Parent = panel

-- Função para criar botões
local function createButton(name, data)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(0, 200, 0, 50)
    button.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
    button.TextColor3 = Color3.new(1, 1, 1)
    button.Text = " " .. name
    button.Font = Enum.Font.GothamBold
    button.TextSize = 18
    button.AutoButtonColor = false
    button.Parent = panel

    local icon = Instance.new("ImageLabel")
    icon.Size = UDim2.new(0, 24, 0, 24)
    icon.Position = UDim2.new(0, 10, 0.5, -12)
    icon.BackgroundTransparency = 1
    icon.Image = data.icon
    icon.Parent = button

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = button

    -- Ação ao clicar
    button.MouseButton1Click:Connect(function()
        clickSound:Play()

        -- Animação de clique
        local tween = TweenService:Create(button, TweenInfo.new(0.1), {
            BackgroundColor3 = Color3.fromRGB(100, 100, 100)
        })
        tween:Play()
        tween.Completed:Connect(function()
            TweenService:Create(button, TweenInfo.new(0.2), {
                BackgroundColor3 = Color3.fromRGB(70, 70, 70)
            }):Play()
        end)

        -- Teleporte
        local character = player.Character or player.CharacterAdded:Wait()
        local hrp = character:WaitForChild("HumanoidRootPart")
        hrp.CFrame = CFrame.new(data.position + Vector3.new(0, 3, 0)) -- levanta um pouco
    end)
end

-- Criar botões para cada posição de teleporte
for name, data in pairs(teleportPositions) do
    createButton(name, data)
end
