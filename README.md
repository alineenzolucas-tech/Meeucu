-- Script Noclip Universal
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local player = Players.LocalPlayer

-- Ativa o Noclip
local noclip = true

RunService.Stepped:Connect(function()
    if noclip then
        local character = player.Character
        if character then
            for _, v in pairs(character:GetDescendants()) do
                if v:IsA("BasePart") then
                    v.CanCollide = false
                end
            end
        end
    end
end)

-- Para desativar o noclip, mude a variável acima para 'false' ou execute o seguinte comando separadamente:
-- noclip = false
