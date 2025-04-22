-- Nuke com contagem regressiva + kill all (Prison Life)
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local LocalPlayer = Players.LocalPlayer

local countdownTime = 3 -- segundos até a explosão

-- Envia mensagem no chat global
local function sendChatMessage(text)
    game.ReplicatedStorage.DefaultChatSystemChatEvents.SayMessageRequest:FireServer(text, "All")
end

-- Encontrar o RemoteEvent de Kill
local killEvent = ReplicatedStorage:WaitForChild("KillEvent")

-- Contagem regressiva
for i = countdownTime, 1, -1 do
    sendChatMessage("[ALERTA] Nuke caindo em " .. i .. " segundos!")
    wait(1)
end

sendChatMessage("**NUKE LANÇADA! TODOS SERÃO ELIMINADOS!**")

-- Executa o kill via remote para todos os jogadores
for _, player in pairs(Players:GetPlayers()) do
    if player ~= LocalPlayer then
        pcall(function()
            killEvent:FireServer(player) -- Dispara o KillEvent para matar o jogador
        end)
    end
end
