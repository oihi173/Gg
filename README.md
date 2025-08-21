local player = game.Players.LocalPlayer
local ReplicatedStorage = game:GetService("ReplicatedStorage")

-- Painel
local screenGui = Instance.new("ScreenGui", player.PlayerGui)
screenGui.Name = "PetPanel"

local frame = Instance.new("Frame", screenGui)
frame.Size = UDim2.new(0, 300, 0, 200)
frame.Position = UDim2.new(0.5, -150, 0.5, -100)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.Active = true
frame.Draggable = true

local closeBtn = Instance.new("TextButton", frame)
closeBtn.Size = UDim2.new(0, 30, 0, 30)
closeBtn.Position = UDim2.new(1, -35, 0, 5)
closeBtn.Text = "X"

closeBtn.MouseButton1Click:Connect(function()
    frame.Visible = false
end)

local openBtn = Instance.new("TextButton", screenGui)
openBtn.Size = UDim2.new(0, 80, 0, 30)
openBtn.Position = UDim2.new(0, 10, 0, 10)
openBtn.Text = "Abrir Painel"
openBtn.MouseButton1Click:Connect(function()
    frame.Visible = true
end)

-- Botões para criar pets
local pet1Btn = Instance.new("TextButton", frame)
pet1Btn.Size = UDim2.new(0, 120, 0, 40)
pet1Btn.Position = UDim2.new(0, 10, 0, 50)
pet1Btn.Text = "Pet 1 (Segue embaixo)"

local pet2Btn = Instance.new("TextButton", frame)
pet2Btn.Size = UDim2.new(0, 120, 0, 40)
pet2Btn.Position = UDim2.new(0, 10, 0, 100)
pet2Btn.Text = "Pet 2 (Cabeça)"

-- Funções dos pets
function spawnPet1()
    local pet = ReplicatedStorage:FindFirstChild("Pet1"):Clone()
    pet.Parent = workspace

    local char = player.Character or player.CharacterAdded:Wait()
    local hrp = char:WaitForChild("HumanoidRootPart")

    pet.PrimaryPart = pet:FindFirstChild("PrimaryPart") or pet:FindFirstChildWhichIsA("BasePart")
    if not pet.PrimaryPart then return end

    -- Atualiza a posição do pet embaixo do chão do jogador
    game:GetService("RunService").RenderStepped:Connect(function()
        if pet and pet.Parent then
            local playerPos = hrp.Position
            pet:SetPrimaryPartCFrame(
                CFrame.new(playerPos.X, playerPos.Y - 6, playerPos.Z)
            )
        end
    end)
end

function spawnPet2()
    local pet = ReplicatedStorage:FindFirstChild("Pet2"):Clone()
    pet.Parent = workspace

    local char = player.Character or player.CharacterAdded:Wait()
    local head = char:WaitForChild("Head")

    pet.PrimaryPart = pet:FindFirstChild("PrimaryPart") or pet:FindFirstChildWhichIsA("BasePart")
    if not pet.PrimaryPart then return end

    -- Atualiza a posição do pet em cima da cabeça do jogador
    game:GetService("RunService").RenderStepped:Connect(function()
        if pet and pet.Parent then
            local headPos = head.Position
            pet:SetPrimaryPartCFrame(
                CFrame.new(headPos.X, headPos.Y + 2.5, headPos.Z)
            )
        end
    end)
end

pet1Btn.MouseButton1Click:Connect(spawnPet1)
pet2Btn.MouseButton1Click:Connect(spawnPet2)
