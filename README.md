-- =========================
-- ANTI PULO + FPS BOOST
-- ÍCONE REDONDO (MOBILE)
-- =========================

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Lighting = game:GetService("Lighting")
local UserSettings = UserSettings()
local player = Players.LocalPlayer

local ativo = false
local humanoid

-- ===== FPS BOOST =====
local function FPSBoost()
    pcall(function()
        settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
        UserSettings.GameSettings.GraphicsQuality = 1
    end)

    Lighting.GlobalShadows = false
    Lighting.FogEnd = 9e9

    for _, v in pairs(game:GetDescendants()) do
        if v:IsA("ParticleEmitter") or v:IsA("Trail")
        or v:IsA("BloomEffect") or v:IsA("BlurEffect")
        or v:IsA("SunRaysEffect") then
            v.Enabled = false
        end
    end
end

-- ===== ANTI PULO =====
local function aplicar()
    if humanoid then
        humanoid.JumpPower = 0
        humanoid:SetStateEnabled(Enum.HumanoidStateType.Jumping, false)
    end
end

local function remover()
    if humanoid then
        humanoid.JumpPower = 50
        humanoid:SetStateEnabled(Enum.HumanoidStateType.Jumping, true)
    end
end

-- ===== PERSONAGEM =====
local function onCharacter(char)
    humanoid = char:WaitForChild("Humanoid")
end

if player.Character then onCharacter(player.Character) end
player.CharacterAdded:Connect(onCharacter)

-- ===== GUI =====
local gui = Instance.new("ScreenGui")
gui.Parent = player.PlayerGui
gui.Name = "IconControl"
gui.ResetOnSpawn = false

local botao = Instance.new("ImageButton")
botao.Parent = gui
botao.Size = UDim2.new(0, 55, 0, 55)
botao.Position = UDim2.new(0, 15, 0.55, 0)
botao.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
botao.Image = "rbxassetid://7072719338" -- ícone simples
botao.AutoButtonColor = true
botao.Active = true
botao.Draggable = true

-- deixa redondo
local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(1, 0)
corner.Parent = botao

-- ===== TOQUE =====
botao.MouseButton1Click:Connect(function()
    ativo = not ativo

    if ativo then
        botao.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
        FPSBoost()
        aplicar()
    else
        botao.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
        remover()
    end
end)

-- ===== GARANTIA =====
RunService.RenderStepped:Connect(function()
    if ativo and humanoid then
        humanoid.JumpPower = 0
    end
end)
