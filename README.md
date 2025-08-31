--[[ 
    oihi_17 hub - Script: el macho (versão mobile-friendly)
    Auto Farm de Cavalos (simulado)
    Criado para servidor privado
--]]

-- Interface do Hub
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "oihi_17_hub"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0.3, 0, 0.15, 0) -- 30% da largura, 15% da altura da tela
Frame.Position = UDim2.new(0.35, 0, 0.8, 0) -- parte inferior central
Frame.AnchorPoint = Vector2.new(0.5, 0.5)
Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Frame.BackgroundTransparency = 0.1
Frame.BorderSizePixel = 0
Frame.Parent = ScreenGui

local FarmButton = Instance.new("TextButton")
FarmButton.Size = UDim2.new(1, 0, 1, 0)
FarmButton.Position = UDim2.new(0, 0, 0, 0)
FarmButton.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
FarmButton.TextColor3 = Color3.new(1, 1, 1)
FarmButton.Font = Enum.Font.GothamBold
FarmButton.TextScaled = true
FarmButton.Text = "AutoFarm Cavalos"
FarmButton.Parent = Frame

-- Auto Farm Lógica (Simulada)
local farming = false

local function startAutoFarm()
    farming = not farming
    FarmButton.Text = farming and "Parar AutoFarm" or "AutoFarm Cavalos"

    while farming do
        local horse = workspace:FindFirstChild("Cavalo") -- Exemplo
        if horse then
            print("Interagindo com:", horse.Name)
            -- Aqui iria o FireServer verdadeiro se você souber o evento
        end
        wait(2)
    end
end

FarmButton.MouseButton1Click:Connect(startAutoFarm)
