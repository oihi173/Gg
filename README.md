-- Script Farm Otimizado (Não Repete Moedas)
-- Coloque no StarterGui ou StarterPlayerScripts

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

-- Configurações
local farmAtivado = false
local noclipAtivado = false
local velocidadeMovimento = 100
local modoMovimento = "Teleport"

-- Sistema anti-repetição
local moedasVisitadas = {}
local tempoLimpeza = 5 -- Limpa cache a cada 5 segundos

-- Limpar cache de moedas visitadas periodicamente
spawn(function()
    while true do
        wait(tempoLimpeza)
        moedasVisitadas = {}
    end
end)

-- Função para encontrar moedas NÃO visitadas ordenadas por distância
local function encontrarMoedasDisponiveis()
    local moedas = {}
    
    for _, obj in pairs(workspace:GetDescendants()) do
        if obj.Name == "Coin_Server" and obj:IsA("BasePart") and obj.Parent then
            -- Verifica se a moeda NÃO foi visitada
            if not moedasVisitadas[obj] then
                local distancia = (humanoidRootPart.Position - obj.Position).Magnitude
                table.insert(moedas, {
                    objeto = obj,
                    distancia = distancia
                })
            end
        end
    end
    
    -- Ordenar por distância (mais próximas primeiro)
    table.sort(moedas, function(a, b)
        return a.distancia < b.distancia
    end)
    
    return moedas
end

-- Função Noclip
local noclipConnection
local function ativarNoclip()
    if noclipConnection then return end
    
    noclipConnection = RunService.Stepped:Connect(function()
        if noclipAtivado then
            for _, parte in pairs(character:GetDescendants()) do
                if parte:IsA("BasePart") then
                    parte.CanCollide = false
                end
            end
        end
    end)
end

local function desativarNoclip()
    if noclipConnection then
        noclipConnection:Disconnect()
        noclipConnection = nil
    end
    
    for _, parte in pairs(character:GetDescendants()) do
        if parte:IsA("BasePart") then
            parte.CanCollide = true
        end
    end
end

-- Função para mover até a moeda
local function moverParaMoeda(moeda)
    if not moeda or not moeda.Parent then return false end
    
    if modoMovimento == "Teleport" then
        humanoidRootPart.CFrame = moeda.CFrame
    else
        local distancia = (humanoidRootPart.Position - moeda.Position).Magnitude
        local tempo = distancia / velocidadeMovimento
        
        local tweenInfo = TweenInfo.new(
            tempo,
            Enum.EasingStyle.Linear,
            Enum.EasingDirection.InOut
        )
        
        local tween = TweenService:Create(
            humanoidRootPart,
            tweenInfo,
            {CFrame = moeda.CFrame}
        )
        
        tween:Play()
        tween.Completed:Wait()
    end
    
    return true
end

-- Loop otimizado SEM repetições
local function loopFarmOtimizado()
    while farmAtivado do
        local moedasDisponiveis = encontrarMoedasDisponiveis()
        
        if #moedasDisponiveis > 0 then
            local moedata = moedasDisponiveis[1]
            local moeda = moedata.objeto
            
            -- Marca como visitada ANTES de ir até ela
            moedasVisitadas[moeda] = true
            
            -- Vai até a moeda
            pcall(function()
                moverParaMoeda(moeda)
            end)
            
            -- Detecta quando a moeda foi destruída (coletada)
            local tentativas = 0
            while moeda and moeda.Parent and tentativas < 10 do
                tentativas = tentativas + 1
                RunService.Heartbeat:Wait()
            end
            
        else
            -- Se não há moedas novas, limpa o cache e tenta novamente
            moedasVisitadas = {}
            wait(0.5)
        end
        
        RunService.Heartbeat:Wait()
    end
end

-- GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "FarmOtimizadoGUI"
screenGui.ResetOnSpawn = false
screenGui.Parent = player.PlayerGui

-- Frame principal
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 320, 0, 420)
mainFrame.Position = UDim2.new(0.02, 0, 0.2, 0)
mainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 20)
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.Parent = screenGui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 12)
corner.Parent = mainFrame

-- Título
local titulo = Instance.new("TextLabel")
titulo.Size = UDim2.new(1, 0, 0, 45)
titulo.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
titulo.Text = "⚡ FARM OTIMIZADO - SEM REPETIÇÃO"
titulo.TextColor3 = Color3.fromRGB(255, 255, 255)
titulo.TextSize = 14
titulo.Font = Enum.Font.GothamBold
titulo.Parent = mainFrame

local tituloCorner = Instance.new("UICorner")
tituloCorner.CornerRadius = UDim.new(0, 12)
tituloCorner.Parent = titulo

-- Botão Farm
local btnFarm = Instance.new("TextButton")
btnFarm.Size = UDim2.new(0, 280, 0, 50)
btnFarm.Position = UDim2.new(0, 20, 0, 60)
btnFarm.BackgroundColor3 = Color3.fromRGB(220, 50, 50)
btnFarm.Text = "▶ INICIAR FARM"
btnFarm.TextColor3 = Color3.fromRGB(255, 255, 255)
btnFarm.TextSize = 18
btnFarm.Font = Enum.Font.GothamBold
btnFarm.Parent = mainFrame

local btnFarmCorner = Instance.new("UICorner")
btnFarmCorner.CornerRadius = UDim.new(0, 10)
btnFarmCorner.Parent = btnFarm

-- Botão Noclip
local btnNoclip = Instance.new("TextButton")
btnNoclip.Size = UDim2.new(0, 280, 0, 45)
btnNoclip.Position = UDim2.new(0, 20, 0, 120)
btnNoclip.BackgroundColor3 = Color3.fromRGB(220, 50, 50)
btnNoclip.Text = "👻 ATIVAR NOCLIP"
btnNoclip.TextColor3 = Color3.fromRGB(255, 255, 255)
btnNoclip.TextSize = 16
btnNoclip.Font = Enum.Font.GothamBold
btnNoclip.Parent = mainFrame

local btnNoclipCorner = Instance.new("UICorner")
btnNoclipCorner.CornerRadius = UDim.new(0, 10)
btnNoclipCorner.Parent = btnNoclip

-- Botão Modo
local btnModo = Instance.new("TextButton")
btnModo.Size = UDim2.new(0, 280, 0, 45)
btnModo.Position = UDim2.new(0, 20, 0, 175)
btnModo.BackgroundColor3 = Color3.fromRGB(220, 50, 220)
btnModo.Text = "⚡ MODO: TELEPORT"
btnModo.TextColor3 = Color3.fromRGB(255, 255, 255)
btnModo.TextSize = 16
btnModo.Font = Enum.Font.GothamBold
btnModo.Parent = mainFrame

local btnModoCorner = Instance.new("UICorner")
btnModoCorner.CornerRadius = UDim.new(0, 10)
btnModoCorner.Parent = btnModo

-- Botão Limpar Cache
local btnLimpar = Instance.new("TextButton")
btnLimpar.Size = UDim2.new(0, 280, 0, 40)
btnLimpar.Position = UDim2.new(0, 20, 0, 230)
btnLimpar.BackgroundColor3 = Color3.fromRGB(255, 150, 50)
btnLimpar.Text = "🔄 LIMPAR CACHE"
btnLimpar.TextColor3 = Color3.fromRGB(255, 255, 255)
btnLimpar.TextSize = 15
btnLimpar.Font = Enum.Font.GothamBold
btnLimpar.Parent = mainFrame

local btnLimparCorner = Instance.new("UICorner")
btnLimparCorner.CornerRadius = UDim.new(0, 10)
btnLimparCorner.Parent = btnLimpar

-- Label de velocidade
local velocidadeLabel = Instance.new("TextLabel")
velocidadeLabel.Size = UDim2.new(0, 280, 0, 25)
velocidadeLabel.Position = UDim2.new(0, 20, 0, 285)
velocidadeLabel.BackgroundTransparency = 1
velocidadeLabel.Text = "⚡ Velocidade Atual: 100"
velocidadeLabel.TextColor3 = Color3.fromRGB(255, 200, 50)
velocidadeLabel.TextSize = 15
velocidadeLabel.Font = Enum.Font.GothamBold
velocidadeLabel.Parent = mainFrame

-- TextBox para digitar velocidade
local velocidadeInput = Instance.new("TextBox")
velocidadeInput.Size = UDim2.new(0, 180, 0, 40)
velocidadeInput.Position = UDim2.new(0, 20, 0, 315)
velocidadeInput.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
velocidadeInput.Text = "100"
velocidadeInput.TextColor3 = Color3.fromRGB(255, 255, 255)
velocidadeInput.TextSize = 18
velocidadeInput.Font = Enum.Font.GothamBold
velocidadeInput.PlaceholderText = "Digite velocidade..."
velocidadeInput.TextXAlignment = Enum.TextXAlignment.Center
velocidadeInput.ClearTextOnFocus = false
velocidadeInput.Parent = mainFrame

local inputCorner = Instance.new("UICorner")
inputCorner.CornerRadius = UDim.new(0, 8)
inputCorner.Parent = velocidadeInput

-- Botão Aplicar Velocidade
local btnAplicar = Instance.new("TextButton")
btnAplicar.Size = UDim2.new(0, 90, 0, 40)
btnAplicar.Position = UDim2.new(0, 210, 0, 315)
btnAplicar.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
btnAplicar.Text = "✓ OK"
btnAplicar.TextColor3 = Color3.fromRGB(255, 255, 255)
btnAplicar.TextSize = 18
btnAplicar.Font = Enum.Font.GothamBold
btnAplicar.Parent = mainFrame

local btnAplicarCorner = Instance.new("UICorner")
btnAplicarCorner.CornerRadius = UDim.new(0, 8)
btnAplicarCorner.Parent = btnAplicar

-- Status
local statusLabel = Instance.new("TextLabel")
statusLabel.Size = UDim2.new(0, 280, 0, 25)
statusLabel.Position = UDim2.new(0, 20, 0, 365)
statusLabel.BackgroundTransparency = 1
statusLabel.Text = "⚪ Aguardando..."
statusLabel.TextColor3 = Color3.fromRGB(180, 180, 180)
statusLabel.TextSize = 14
statusLabel.Font = Enum.Font.GothamBold
statusLabel.Parent = mainFrame

-- Info
local infoLabel = Instance.new("TextLabel")
infoLabel.Size = UDim2.new(0, 280, 0, 20)
infoLabel.Position = UDim2.new(0, 20, 0, 390)
infoLabel.BackgroundTransparency = 1
infoLabel.Text = "💰 Moedas disponíveis: 0"
infoLabel.TextColor3 = Color3.fromRGB(255, 215, 0)
infoLabel.TextSize = 13
infoLabel.Font = Enum.Font.GothamBold
infoLabel.Parent = mainFrame

-- Função do botão Aplicar Velocidade
btnAplicar.MouseButton1Click:Connect(function()
    local valor = tonumber(velocidadeInput.Text)
    
    if valor and valor > 0 and valor <= 1000 then
        velocidadeMovimento = valor
        velocidadeLabel.Text = string.format("⚡ Velocidade Atual: %d", velocidadeMovimento)
        
        btnAplicar.BackgroundColor3 = Color3.fromRGB(50, 255, 50)
        btnAplicar.Text = "✓"
        wait(0.2)
        btnAplicar.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
        btnAplicar.Text = "✓ OK"
    else
        btnAplicar.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
        btnAplicar.Text = "✗"
        wait(0.3)
        btnAplicar.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
        btnAplicar.Text = "✓ OK"
        velocidadeInput.Text = tostring(velocidadeMovimento)
    end
end)

-- Enter no TextBox
velocidadeInput.FocusLost:Connect(function(enterPressed)
    if enterPressed then
        btnAplicar.MouseButton1Click:Fire()
    end
end)

-- Função do botão Limpar Cache
btnLimpar.MouseButton1Click:Connect(function()
    moedasVisitadas = {}
    btnLimpar.BackgroundColor3 = Color3.fromRGB(50, 255, 50)
    btnLimpar.Text = "✓ LIMPO!"
    wait(0.5)
    btnLimpar.BackgroundColor3 = Color3.fromRGB(255, 150, 50)
    btnLimpar.Text = "🔄 LIMPAR CACHE"
end)

-- Função do botão Farm
btnFarm.MouseButton1Click:Connect(function()
    farmAtivado = not farmAtivado
    
    if farmAtivado then
        btnFarm.BackgroundColor3 = Color3.fromRGB(50, 220, 50)
        btnFarm.Text = "⏸ PAUSAR"
        statusLabel.Text = "🟢 FARM ATIVO - SEM REPETIR!"
        statusLabel.TextColor3 = Color3.fromRGB(50, 255, 50)
        
        spawn(loopFarmOtimizado)
    else
        btnFarm.BackgroundColor3 = Color3.fromRGB(220, 50, 50)
        btnFarm.Text = "▶ INICIAR FARM"
        statusLabel.Text = "🔴 PAUSADO"
        statusLabel.TextColor3 = Color3.fromRGB(220, 50, 50)
    end
end)

-- Função do botão Noclip
btnNoclip.MouseButton1Click:Connect(function()
    noclipAtivado = not noclipAtivado
    
    if noclipAtivado then
        btnNoclip.BackgroundColor3 = Color3.fromRGB(50, 220, 50)
        btnNoclip.Text = "👻 NOCLIP ON"
        ativarNoclip()
    else
        btnNoclip.BackgroundColor3 = Color3.fromRGB(220, 50, 50)
        btnNoclip.Text = "👻 ATIVAR NOCLIP"
        desativarNoclip()
    end
end)

-- Função do botão Modo
btnModo.MouseButton1Click:Connect(function()
    if modoMovimento == "Teleport" then
        modoMovimento = "Tween"
        btnModo.Text = "✈️ MODO: VOAR"
        btnModo.BackgroundColor3 = Color3.fromRGB(50, 120, 220)
    else
        modoMovimento = "Teleport"
        btnModo.Text = "⚡ MODO: TELEPORT"
        btnModo.BackgroundColor3 = Color3.fromRGB(220, 50, 220)
    end
end)

-- Atualizar informações
spawn(function()
    while wait(0.3) do
        local moedas = encontrarMoedasDisponiveis()
        infoLabel.Text = string.format("💰 Moedas disponíveis: %d", #moedas)
    end
end)

-- Atualizar character
player.CharacterAdded:Connect(function(newChar)
    character = newChar
    humanoid = character:WaitForChild("Humanoid")
    humanoidRootPart = character:WaitForChild("HumanoidRootPart")
    
    moedasVisitadas = {} -- Limpa cache ao respawnar
    
    if noclipAtivado then
        if noclipConnection then
            noclipConnection:Disconnect()
        end
        ativarNoclip()
    end
end)
