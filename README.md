-- Script para "Xerife vs Assassino" no Roblox
-- Este script adiciona um botão na tela para ligar/desligar a hitbox dos inimigos
-- A hitbox dos inimigos fica vermelha e maior quando ativada

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer
local ScreenGui = Instance.new("ScreenGui")
local ToggleButton = Instance.new("TextButton")

-- Configurações do botão
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
ToggleButton.Parent = ScreenGui
ToggleButton.Size = UDim2.new(0, 100, 0, 50)
ToggleButton.Position = UDim2.new(0, 10, 0, 10)
ToggleButton.Text = "Ativar Hitbox"
ToggleButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)

local hitboxEnabled = false

-- Função para alternar a hitbox
local function toggleHitbox()
    hitboxEnabled = not hitboxEnabled
    ToggleButton.Text = hitboxEnabled and "Desativar Hitbox" or "Ativar Hitbox"
    
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer then
            local character = player.Character
            if character then
                for _, part in pairs(character:GetChildren()) do
                    if part:IsA("BasePart") then
                        if hitboxEnabled then
                            part.Size = part.Size * 2
                            part.Color = Color3.fromRGB(255, 0, 0)
                        else
                            part.Size = part.Size / 2
                            part.Color = Color3.fromRGB(255, 255, 255)
                        end
                    end
                end
            end
        end
    end
end

ToggleButton.MouseButton1Click:Connect(toggleHitbox)

-- Função para garantir que o jogo não trave
RunService.Heartbeat:Connect(function()
    if hitboxEnabled then
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= LocalPlayer then
                local character = player.Character
                if character then
                    for _, part in pairs(character:GetChildren()) do
                        if part:IsA("BasePart") then
                            part.Size = Vector3.new(4, 4, 4) -- Ajuste o tamanho conforme necessário
                            part.Color = Color3.fromRGB(255, 0, 0)
                        end
                    end
                end
            end
        end
    end
end)
