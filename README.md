loadstring([[
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Communication = ReplicatedStorage:WaitForChild("Communication")
local Events = Communication:WaitForChild("Events")

-- Criar GUI
local player = game.Players.LocalPlayer
local screenGui = Instance.new("ScreenGui", player.PlayerGui)
screenGui.ResetOnSpawn = false

local toggleBtn = Instance.new("TextButton", screenGui)
toggleBtn.Size = UDim2.new(0, 150, 0, 50)
toggleBtn.Position = UDim2.new(0.5, -75, 0.8, 0)
toggleBtn.Text = "Ativar Farm"
toggleBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
toggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleBtn.Font = Enum.Font.SourceSansBold
toggleBtn.TextSize = 20
toggleBtn.AutoButtonColor = true
toggleBtn.Visible = true
toggleBtn.Active = true

-- Controle
local ativo = false

local function usarItem(item)
    local args = {"Use", item}
    Events:WaitForChild(""):FireServer(unpack(args))
end

-- Loop
task.spawn(function()
    while true do
        task.wait(0.1)
        if ativo then
            usarItem("Lasso")
            usarItem("Harvester")
        end
    end
end)

-- Toggle botão
toggleBtn.MouseButton1Click:Connect(function()
    ativo = not ativo
    if ativo then
        toggleBtn.Text = "Desativar Farm"
        toggleBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
    else
        toggleBtn.Text = "Ativar Farm"
        toggleBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
    end
end)
]])
