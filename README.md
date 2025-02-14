-- Void FPS Booster + FPS & Ping Display com efeito Chroma e Barra UI
local Players = game:GetService("Players")
local Lighting = game:GetService("Lighting")
local Stats = game:GetService("Stats")
local RunService = game:GetService("RunService")

-- Otimizações de FPS
Lighting.GlobalShadows = false
Lighting.FogEnd = 1e10
Lighting.Brightness = 1
Lighting.ClockTime = 14 -- Iluminação neutra para melhor visibilidade

-- Reduzindo detalhes gráficos para aumentar o FPS
for _, v in pairs(workspace:GetDescendants()) do
    if v:IsA("BasePart") then
        v.Material = Enum.Material.Plastic
        v.Reflectance = 0
    elseif v:IsA("Decal") or v:IsA("Texture") then
        v:Destroy() -- Remove texturas para melhorar o desempenho
    end
end

for _, v in pairs(game:GetDescendants()) do
    if v:IsA("ParticleEmitter") or v:IsA("Trail") or v:IsA("Smoke") or v:IsA("Sparkles") then
        v:Destroy()
    end
end

-- Criando interface para FPS, Ping e nome do script
local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local FPSLabel = Instance.new("TextLabel")
local PingLabel = Instance.new("TextLabel")
local VoidLabel = Instance.new("TextLabel") -- Nome do script

ScreenGui.Parent = game:GetService("CoreGui")

-- Criando a Barra UI
Frame.Parent = ScreenGui
Frame.Position = UDim2.new(0.01, 0, 0.85, 0) -- Posição inferior esquerda
Frame.Size = UDim2.new(0, 250, 0, 40) -- Tamanho reduzido
Frame.BackgroundColor3 = Color3.new(0, 0, 0)
Frame.BackgroundTransparency = 0.3
Frame.BorderSizePixel = 0

-- Exibição do FPS
FPSLabel.Parent = Frame
FPSLabel.Position = UDim2.new(0, 10, 0, 5)
FPSLabel.Size = UDim2.new(0, 100, 0, 30)
FPSLabel.TextColor3 = Color3.new(1, 1, 1)
FPSLabel.BackgroundTransparency = 1
FPSLabel.TextSize = 20
FPSLabel.Text = "FPS: --"

-- Exibição do Ping
PingLabel.Parent = Frame
PingLabel.Position = UDim2.new(0, 120, 0, 5)
PingLabel.Size = UDim2.new(0, 100, 0, 30)
PingLabel.TextColor3 = Color3.new(1, 1, 1)
PingLabel.BackgroundTransparency = 1
PingLabel.TextSize = 20
PingLabel.Text = "Ping: --"

-- Nome do script (marca d’água discreta)
VoidLabel.Parent = Frame
VoidLabel.Position = UDim2.new(0.5, -30, 1, -15) -- Fica na parte inferior da barra
VoidLabel.Size = UDim2.new(0, 100, 0, 15)
VoidLabel.TextColor3 = Color3.new(1, 1, 1) -- Branco neutro
VoidLabel.BackgroundTransparency = 1
VoidLabel.TextSize = 14
VoidLabel.Text = "Void FPS Booster"

-- Função para efeito Chroma (RGB animado)
local function ChromaEffect()
    local t = 0
    while true do
        local r = math.sin(t) * 0.5 + 0.5
        local g = math.sin(t + 2) * 0.5 + 0.5
        local b = math.sin(t + 4) * 0.5 + 0.5
        local color = Color3.new(r, g, b)
        
        FPSLabel.TextColor3 = color
        PingLabel.TextColor3 = color
        
        t = t + 0.05
        wait(0.1)
    end
end

-- Atualizando FPS e Ping
RunService.RenderStepped:Connect(function()
    local FPS = math.floor(1 / RunService.RenderStepped:Wait())
    local Ping = Stats.Network.ServerStatsItem["Data Ping"]:GetValue()
    
    FPSLabel.Text = "FPS: " .. FPS
    PingLabel.Text = "Ping: " .. math.floor(Ping) .. " ms"
end)

-- Iniciar efeito Chroma
spawn(ChromaEffect)

print("Void FPS Booster ativado com sucesso!")
