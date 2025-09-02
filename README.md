loadstring([[
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local hrp = character:WaitForChild("HumanoidRootPart")
local camera = workspace.CurrentCamera

local flying = false
local speed = 50
local boostMult = 2.5
local ascendSpeed = 40

local bv
local bg
local moveVector = Vector3.new(0,0,0)
local moveForward, moveRight, moveUp, isBoost = 0,0,0,false

-- Criar GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = game:GetService("CoreGui")

local FlyButton = Instance.new("TextButton")
FlyButton.Size = UDim2.new(0,120,0,50)
FlyButton.Position = UDim2.new(0.05,0,0.8,0)
FlyButton.BackgroundColor3 = Color3.fromRGB(30,30,30)
FlyButton.TextColor3 = Color3.fromRGB(255,255,255)
FlyButton.TextScaled = true
FlyButton.Text = "FLY: OFF"
FlyButton.Parent = ScreenGui
FlyButton.Visible = true
FlyButton.Active = true
FlyButton.Draggable = true

local function createControllers()
    bv = Instance.new("BodyVelocity")
    bv.MaxForce = Vector3.new(1e5, 1e5, 1e5)
    bv.Velocity = Vector3.new(0,0,0)
    bv.P = 1e4
    bv.Parent = hrp

    bg = Instance.new("BodyGyro")
    bg.MaxTorque = Vector3.new(1e5, 1e5, 1e5)
    bg.CFrame = hrp.CFrame
    bg.P = 1e4
    bg.Parent = hrp
end

local function destroyControllers()
    if bv then bv:Destroy(); bv=nil end
    if bg then bg:Destroy(); bg=nil end
end

local function updateMove()
    local camCFrame = camera.CFrame
    local look = Vector3.new(camCFrame.LookVector.X, 0, camCFrame.LookVector.Z).Unit
    if look ~= look then look = Vector3.new(0,0,-1) end
    local right = camCFrame.RightVector
    right = Vector3.new(right.X, 0, right.Z).Unit

    local horizontal = (look * moveForward) + (right * moveRight)
    if horizontal.Magnitude > 1 then horizontal = horizontal.Unit end

    local currentSpeed = speed * (isBoost and boostMult or 1)
    moveVector = horizontal * currentSpeed + Vector3.new(0, moveUp * ascendSpeed, 0)
end

-- Controles mobile: botão liga/desliga
FlyButton.MouseButton1Click:Connect(function()
    flying = not flying
    if flying then
        createControllers()
        FlyButton.Text = "FLY: ON"
        FlyButton.BackgroundColor3 = Color3.fromRGB(0,150,0)
    else
        destroyControllers()
        FlyButton.Text = "FLY: OFF"
        FlyButton.BackgroundColor3 = Color3.fromRGB(150,0,0)
    end
end)

-- Teclado (se usar em PC também funciona)
UserInputService.InputBegan:Connect(function(input, gp)
    if gp then return end
    if input.UserInputType == Enum.UserInputType.Keyboard then
        local k = input.KeyCode
        if k == Enum.KeyCode.W then moveForward = 1
        elseif k == Enum.KeyCode.S then moveForward = -1
        elseif k == Enum.KeyCode.A then moveRight = -1
        elseif k == Enum.KeyCode.D then moveRight = 1
        elseif k == Enum.KeyCode.Space then moveUp = 1
        elseif k == Enum.KeyCode.LeftControl or k == Enum.KeyCode.C then moveUp = -1
        elseif k == Enum.KeyCode.LeftShift then isBoost = true
        end
        updateMove()
    end
end)

UserInputService.InputEnded:Connect(function(input, gp)
    if gp then return end
    if input.UserInputType == Enum.UserInputType.Keyboard then
        local k = input.KeyCode
        if k == Enum.KeyCode.W or k == Enum.KeyCode.S then moveForward = 0
        elseif k == Enum.KeyCode.A or k == Enum.KeyCode.D then moveRight = 0
        elseif k == Enum.KeyCode.Space or k == Enum.KeyCode.LeftControl or k == Enum.KeyCode.C then moveUp = 0
        elseif k == Enum.KeyCode.LeftShift then isBoost = false
        end
        updateMove()
    end
end)

RunService.RenderStepped:Connect(function()
    if flying and bv and bg then
        local camCFrame = camera.CFrame
        local targetCFrame = CFrame.new(hrp.Position, hrp.Position + camCFrame.LookVector)
        bg.CFrame = targetCFrame
        bv.Velocity = moveVector
    end
end)

player.CharacterAdded:Connect(function(char)
    character = char
    hrp = character:WaitForChild("HumanoidRootPart")
    destroyControllers()
    flying = false
    FlyButton.Text = "FLY: OFF"
    FlyButton.BackgroundColor3 = Color3.fromRGB(30,30,30)
end)
]])
