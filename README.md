-- Fly Script com Botão Mobile
-- Criado do zero (não é cópia direta do Infinite Yield)

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local player = Players.LocalPlayer
local mouse = player:GetMouse()

local flyEnabled = false
local speed = 60
local ascendSpeed = 50
local bv, bg

-- Criar botão na tela
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "FlyUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game.CoreGui or player:WaitForChild("PlayerGui")

local Button = Instance.new("TextButton")
Button.Size = UDim2.new(0,150,0,50)
Button.Position = UDim2.new(0.05,0,0.8,0)
Button.Text = "Fly OFF"
Button.BackgroundColor3 = Color3.fromRGB(30,30,30)
Button.TextColor3 = Color3.fromRGB(255,255,255)
Button.Font = Enum.Font.SourceSansBold
Button.TextSize = 24
Button.Parent = ScreenGui
Button.Active = true
Button.Draggable = true -- pode arrastar o botão

-- Criação dos controladores
local function createControllers(hrp)
    bv = Instance.new("BodyVelocity")
    bv.MaxForce = Vector3.new(1e5,1e5,1e5)
    bv.P = 1250
    bv.Parent = hrp

    bg = Instance.new("BodyGyro")
    bg.MaxTorque = Vector3.new(1e5,1e5,1e5)
    bg.P = 3000
    bg.Parent = hrp
end

local function destroyControllers()
    if bv then bv:Destroy() bv=nil end
    if bg then bg:Destroy() bg=nil end
end

-- Alternar fly
local function toggleFly()
    flyEnabled = not flyEnabled
    if flyEnabled then
        Button.Text = "Fly ON"
    else
        Button.Text = "Fly OFF"
        destroyControllers()
    end
end

Button.MouseButton1Click:Connect(toggleFly)

-- Movimento
RunService.RenderStepped:Connect(function()
    local char = player.Character
    if not char then return end
    local hrp = char:FindFirstChild("HumanoidRootPart")
    local humanoid = char:FindFirstChildOfClass("Humanoid")
    local cam = workspace.CurrentCamera

    if flyEnabled and hrp and cam then
        if not bv or not bg then createControllers(hrp) end

        local dir = Vector3.new(0,0,0)
        if UserInputService:IsKeyDown(Enum.KeyCode.W) then
            dir = dir + cam.CFrame.LookVector
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.S) then
            dir = dir - cam.CFrame.LookVector
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.A) then
            dir = dir - cam.CFrame.RightVector
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.D) then
            dir = dir + cam
