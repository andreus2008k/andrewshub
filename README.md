-- Configurações
local distanciaMaxima = 50
local tempoDeEspera = 0.5
local numFrutasPorCiclo = 10
local nivelDesejado = 100
local raidDesejado = "Sea King"

-- Função para coletar frutas
local function coletarFrutas()
    for _, fruit in pairs(game.Workspace:GetDescendants()) do
        if fruit.Name == "Fruit" then
            if (game.Players.LocalPlayer.Character.HumanoidRootPart.Position - fruit.Position).Magnitude <= distanciaMaxima then
                game.ReplicatedStorage.ColetarFruit:FireServer(fruit)
                wait(tempoDeEspera)
            end
        end
    end
end

-- Função para farmar níveis
local function farmarNiveis()
    while game.Players.LocalPlayer.Level < nivelDesejado do
        coletarFrutas()
        game.ReplicatedStorage.FarmNivel:FireServer()
        wait(1)
    end
end

-- Função para fazer raids
local function fazerRaid()
    game.ReplicatedStorage.IniciarRaid:FireServer(raidDesejado)
    while game.Players.LocalPlayer.Raid ~= raidDesejado do
        wait(1)
    end
    game.ReplicatedStorage.FinalizarRaid:FireServer()
end

-- Função para coletar caixas
local function coletarCaixas()
    for _, caixa in pairs(game.Workspace:GetDescendants()) do
        if caixa.Name == "Chest" then
            if (game.Players.LocalPlayer.Character.HumanoidRootPart.Position - caixa.Position).Magnitude <= distanciaMaxima then
                game.ReplicatedStorage.ColetarChest:FireServer(caixa)
                wait(tempoDeEspera)
            end
        end
    end
end

-- Função para coletar recursos
local function coletarRecursos()
    for _, recurso in pairs(game.Workspace:GetDescendants()) do
        if recurso.Name == "Resource" then
            if (game.Players.LocalPlayer.Character.HumanoidRootPart.Position - recurso.Position).Magnitude <= distanciaMaxima then
                game.ReplicatedStorage.ColetarResource:FireServer(recurso)
                wait(tempoDeEspera)
            end
        end
    end
end

-- Função para comprar itens
local function comprarItens()
    game.ReplicatedStorage.ComprarItem:FireServer("Sword")
    game.ReplicatedStorage.ComprarItem:FireServer("Shield")
    game.ReplicatedStorage.ComprarItem:FireServer("Armor")
end

-- Loop principal
while true do
    if game.Players.LocalPlayer.Level < nivelDesejado then
        farmarNiveis()
    else
        fazerRaid()
        coletarCaixas()
        coletarRecursos()
        comprarItens()
    end
    wait(1)
end
