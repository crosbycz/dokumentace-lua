# dokumentace-lua
Česká dokumentace Lua


# Roblox Studio & Lua - Komplexní průvodce 🎮

[![Roblox Developer](https://img.shields.io/badge/Roblox-Developer-red.svg)](https://developer.roblox.com)
[![Lua](https://img.shields.io/badge/Lua-5.1-blue.svg)](https://www.lua.org)

## 📋 Obsah
1. [Základy Roblox Studia](#základy-roblox-studia)
2. [Lua Programování](#lua-programování)
3. [Roblox API](#roblox-api)
4. [Pokročilé techniky](#pokročilé-techniky)
5. [Optimalizace a výkon](#optimalizace-a-výkon)
6. [Příklady projektů](#příklady-projektů)

## Základy Roblox Studia

### Uživatelské rozhraní
- **Explorer Window**: Hierarchický přehled všech objektů ve hře
- **Properties Window**: Nastavení vlastností vybraného objektu
- **Toolbar**: Nástroje pro manipulaci s objekty
- **Output Window**: Konzole pro debugging a chybové zprávy

### Základní nástroje
```
🔧 Part Tool - Vytváření základních tvarů
🎨 Material Tool - Změna materiálů objektů
📏 Scale Tool - Změna velikosti objektů
🔄 Rotate Tool - Rotace objektů
🔗 Weld Tool - Spojování objektů
```

### Workspace hierarchie
```lua
game
├── Workspace
│   ├── Parts
│   ├── Models
│   └── Scripts
├── ReplicatedStorage
├── ServerScriptService
└── StarterPlayer
```

## Lua Programování

### Základní syntaxe
```lua
-- Proměnné
local číslo = 42
local text = "Hello Roblox"
local jeAktivní = true

-- Podmínky
if číslo > 40 then
    print("Větší než 40")
else
    print("Menší nebo rovno 40")
end

-- Cykly
for i = 1, 10 do
    print(i)
end

while jeAktivní do
    -- kód
end

-- Funkce
local function pozdrav(jméno)
    return "Ahoj " .. jméno
end
```

### Práce s tabulkami
```lua
-- Vytvoření tabulky
local inventář = {
    "meč",
    "štít",
    "lektvar"
}

-- Přidání prvku
table.insert(inventář, "přilba")

-- Odstranění prvku
table.remove(inventář, 1)

-- Procházení tabulky
for index, item in ipairs(inventář) do
    print(index, item)
end
```

## Roblox API

### Instance Management
```lua
-- Vytvoření nové části
local část = Instance.new("Part")
část.Position = Vector3.new(0, 10, 0)
část.Parent = workspace

-- Najít objekt
local model = workspace:FindFirstChild("Model")

-- Smazání objektu
část:Destroy()
```

### Events a připojení
```lua
-- Připojení funkce k události
část.Touched:Connect(function(hit)
    print("Někdo se dotkl části!")
end)

-- Vlastní události
local Events = game:GetService("ReplicatedStorage")
local mojeUdálost = Instance.new("RemoteEvent", Events)
```

### Fyzika a pohyb
```lua
-- Přidání síly
local function aplikujSílu(část)
    local bodyVelocity = Instance.new("BodyVelocity")
    bodyVelocity.Velocity = Vector3.new(0, 50, 0)
    bodyVelocity.Parent = část
end

-- Detekce kolizí
část.Touched:Connect(function(hit)
    local humanoid = hit.Parent:FindFirstChild("Humanoid")
    if humanoid then
        humanoid.Health -= 10
    end
end)
```

## Pokročilé techniky

### Serverové skripty
```lua
-- ServerScriptService
local DataStoreService = game:GetService("DataStoreService")
local store = DataStoreService:GetDataStore("HráčData")

game.Players.PlayerAdded:Connect(function(player)
    local data = store:GetAsync(player.UserId)
    -- Načtení dat hráče
end)
```

### Klientské skripty
```lua
-- StarterPlayerScripts
local UserInputService = game:GetService("UserInputService")

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if input.KeyCode == Enum.KeyCode.Space then
        -- Skok
    end
end)
```

### Replikace dat
```lua
-- Komunikace server-klient
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local remoteEvent = Instance.new("RemoteEvent")
remoteEvent.Name = "MojeUdálost"
remoteEvent.Parent = ReplicatedStorage

-- Server
remoteEvent.OnServerEvent:Connect(function(player, data)
    -- Zpracování dat od klienta
end)

-- Klient
remoteEvent:FireServer("data")
```

## Optimalizace a výkon

### Best Practices
```lua
-- Cachování služeb
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")

-- Používání lokálních proměnných
local function optimalizovanáFunkce()
    local část = workspace.Část
    for i = 1, 100 do
        část.Position = část.Position + Vector3.new(0, 0.1, 0)
    end
end
```

### Redukce síťového provozu
```lua
-- Throttling
local posledníAktualizace = tick()
local COOLDOWN = 0.1

local function aktualizujPozici()
    if tick() - posledníAktualizace >= COOLDOWN then
        -- Aktualizace
        posledníAktualizace = tick()
    end
end
```

## Příklady projektů

### Jednoduchý systém inventáře
```lua
local InventářSystem = {}
InventářSystem.Items = {}

function InventářSystem.PřidejItem(item)
    table.insert(InventářSystem.Items, item)
    -- Aktualizace UI
end

function InventářSystem.OdeberItem(index)
    table.remove(InventářSystem.Items, index)
    -- Aktualizace UI
end

return InventářSystem
```

### Systém zdraví
```lua
local ZdravíSystem = {}

function ZdravíSystem.Init(player)
    local humanoid = player.Character:WaitForChild("Humanoid")
    humanoid.MaxHealth = 100
    humanoid.Health = 100
    
    humanoid.HealthChanged:Connect(function(health)
        -- Aktualizace UI zdraví
    end)
end

return ZdravíSystem
```

### Herní menu
```lua
local MenuSystem = {}

function MenuSystem.VytvořTlačítko(text, pozice)
    local tlačítko = Instance.new("TextButton")
    tlačítko.Text = text
    tlačítko.Position = pozice
    tlačítko.Parent = script.Parent
    
    return tlačítko
end

function MenuSystem.Init()
    local hrátTlačítko = MenuSystem.VytvořTlačítko("Hrát", UDim2.new(0.5, 0, 0.5, 0))
    hrátTlačítko.MouseButton1Click:Connect(function()
        -- Start hry
    end)
end

return MenuSystem
```

## Užitečné tipy a triky

### Debug techniky
```lua
-- Vizuální debug
local function debugRay(origin, direction)
    local ray = Instance.new("Part")
    ray.Size = Vector3.new(0.1, 0.1, direction.Magnitude)
    ray.CFrame = CFrame.new(origin + direction/2, origin + direction)
    ray.Anchored = true
    ray.Parent = workspace
    game:GetService("Debris"):AddItem(ray, 1)
end

-- Logování
local function log(...)
    if game:GetService("RunService"):IsStudio() then
        print(...)
    end
end
```

### Časté chyby a jejich řešení
```lua
-- Problém: Nil reference
local function bezpečnéVolání()
    local success, error = pcall(function()
        -- Potenciálně nebezpečný kód
    end)
    
    if not success then
        warn("Chyba: " .. error)
    end
end

-- Problém: Nekonečné smyčky
local function bezpečnáSmyčka()
    local maxIterací = 1000
    local i = 0
    
    while podmínka do
        i += 1
        if i > maxIterací then
            warn("Detekována potenciální nekonečná smyčka")
            break
        end
    end
end
```

### Výkonnostní monitorování
```lua
local function měřVýkon(funkce)
    local začátek = tick()
    funkce()
    local konec = tick()
    
    return konec - začátek
end

-- Použití
local čas = měřVýkon(function()
    -- Testovaný kód
end)
print("Funkce trvala: " .. čas .. " sekund")
```

## Závěr

Tato dokumentace poskytuje základní i pokročilé koncepty pro vývoj her v Roblox Studio s využitím programovacího jazyka Lua. Pro další informace doporučujeme:

- [Oficiální Roblox dokumentace](https://developer.roblox.com/en-us/)
- [Lua Reference Manual](https://www.lua.org/manual/5.1/)
- [Roblox Developer Forum](https://devforum.roblox.com/)

### Příští kroky
1. Experimentujte s kódem
2. Vytvořte malé testovací projekty
3. Zapojte se do Roblox vývojářské komunity
4. Sdílejte své výtvory a získávejte zpětnou vazbu
