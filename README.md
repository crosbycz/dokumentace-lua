


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


# Dokumentace programovacího jazyka Lua

## Obsah
1. [Úvod do Lua](#úvod-do-lua)
2. [Základní syntaxe](#základní-syntaxe)
3. [Datové typy](#datové-typy)
4. [Proměnné](#proměnné)
5. [Operátory](#operátory)
6. [Řídící struktury](#řídící-struktury)
7. [Funkce](#funkce)
8. [Tabulky](#tabulky)
9. [Objektově orientované programování](#objektově-orientované-programování)
10. [Práce se soubory](#práce-se-soubory)

## Úvod do Lua

Lua je lehký, vysoce přenosný skriptovací jazyk navržený pro vestavěné použití v aplikacích. Je implementován jako knihovna napsaná v čistém C.

### Charakteristické rysy:
- Jednoduchá procedurální syntaxe
- Inkrementální garbage collection
- Dynamické typování
- První třídní funkce s lexikálním scopingem
- Tabulky jako jediná datová struktura

## Základní syntaxe

### Komentáře
```lua
-- Jednořádkový komentář

--[[
    Víceřádkový
    komentář
--]]
```

### Příkazy
- Příkazy mohou být odděleny středníkem (;) nebo novým řádkem
- Poslední středník je volitelný
```lua
print("Hello") print("World")
print("Hello"); print("World");
```

## Datové typy

Lua má 8 základních typů:

1. **nil** - reprezentuje absenci hodnoty
```lua
local prázdná = nil
```

2. **boolean** - true nebo false
```lua
local pravda = true
local nepravda = false
```

3. **number** - reprezentuje reálná čísla
```lua
local celé_číslo = 42
local desetinné_číslo = 3.14
local vědecká_notace = 1.2e-3
```

4. **string** - textový řetězec
```lua
local text1 = "Hello"
local text2 = 'World'
local víceřádkový = [[
    Tento text
    má více
    řádků
]]
```

5. **function** - první třídní hodnota reprezentující funkci
```lua
local funkce = function(x) return x * 2 end
```

6. **userdata** - reprezentuje C data
```lua
-- Používáno hlavně pro interakci s C
```

7. **thread** - reprezentuje nezávislé vlákno výpočtu
```lua
local ko = coroutine.create(function() end)
```

8. **table** - asociativní pole
```lua
local tabulka = {
    klíč = "hodnota",
    [1] = "první"
}
```

## Proměnné

### Deklarace proměnných
```lua
-- Globální proměnná
globální = 10

-- Lokální proměnná
local lokální = 20

-- Více proměnných najednou
local a, b, c = 1, 2, 3
```

### Rozsah platnosti
```lua
-- Globální rozsah
x = 10

do
    -- Lokální rozsah
    local x = 20
    print(x)  -- vypíše 20
end

print(x)  -- vypíše 10
```

## Operátory

### Aritmetické operátory
```lua
local a = 10
local b = 3

print(a + b)  -- sčítání
print(a - b)  -- odčítání
print(a * b)  -- násobení
print(a / b)  -- dělení
print(a % b)  -- modulo
print(a ^ b)  -- mocnina
print(-a)     -- negace
```

### Porovnávací operátory
```lua
print(a == b)  -- rovnost
print(a ~= b)  -- nerovnost
print(a < b)   -- menší než
print(a > b)   -- větší než
print(a <= b)  -- menší nebo rovno
print(a >= b)  -- větší nebo rovno
```

### Logické operátory
```lua
print(true and false)  -- logické AND
print(true or false)   -- logické OR
print(not true)        -- logické NOT
```

### Operátor zřetězení
```lua
print("Hello " .. "World")  -- zřetězení řetězců
```

## Řídící struktury

### if podmínka
```lua
if podmínka then
    -- kód
elseif jiná_podmínka then
    -- kód
else
    -- kód
end
```

### while cyklus
```lua
while podmínka do
    -- kód
end
```

### repeat cyklus
```lua
repeat
    -- kód
until podmínka
```

### for cyklus
```lua
-- Numerický for
for i = 1, 10, 2 do  -- od 1 do 10 s krokem 2
    print(i)
end

-- Generický for
for k, v in pairs(tabulka) do
    print(k, v)
end
```

### break
```lua
while true do
    if podmínka then
        break
    end
end
```

## Funkce

### Definice funkce
```lua
-- Základní syntax
function jméno(parametr1, parametr2)
    return výsledek
end

-- Alternativní syntax
local jméno = function(parametr1, parametr2)
    return výsledek
end
```

### Parametry a návratové hodnoty
```lua
-- Více parametrů
function součet(a, b, c)
    return a + b + c
end

-- Variabilní počet parametrů
function průměr(...)
    local args = {...}
    local suma = 0
    for _, hodnota in ipairs(args) do
        suma = suma + hodnota
    end
    return suma / #args
end

-- Více návratových hodnot
function vícenásobný()
    return 1, 2, 3
end
```

### Uzávěry (Closures)
```lua
function počítadlo()
    local počet = 0
    return function()
        počet = počet + 1
        return počet
    end
end

local mojePočítadlo = počítadlo()
print(mojePočítadlo())  -- 1
print(mojePočítadlo())  -- 2
```

## Tabulky

### Vytvoření a přístup
```lua
-- Vytvoření tabulky
local tabulka = {
    klíč = "hodnota",
    [1] = "první",
    ["text"] = 123
}

-- Přístup k prvkům
print(tabulka.klíč)
print(tabulka[1])
print(tabulka["text"])
```

### Metody pro práci s tabulkami
```lua
-- Vložení prvku
table.insert(tabulka, "nový")

-- Odstranění prvku
table.remove(tabulka, 1)

-- Spojení tabulek
table.concat(tabulka, ", ")

-- Seřazení
table.sort(tabulka)
```

### Iterace přes tabulku
```lua
-- Přes všechny prvky
for k, v in pairs(tabulka) do
    print(k, v)
end

-- Přes číselné indexy
for i, v in ipairs(tabulka) do
    print(i, v)
end
```

## Objektově orientované programování

### Třídy a objekty
```lua
-- Definice třídy
local Osoba = {}
Osoba.__index = Osoba

function Osoba.new(jméno, věk)
    local self = setmetatable({}, Osoba)
    self.jméno = jméno
    self.věk = věk
    return self
end

-- Metody
function Osoba:představSe()
    return string.format("Jmenuji se %s a je mi %d let", self.jméno, self.věk)
end

-- Použití
local jan = Osoba.new("Jan", 30)
print(jan:představSe())
```

### Dědičnost
```lua
-- Rodičovská třída
local Zvíře = {}
Zvíře.__index = Zvíře

function Zvíře.new(jméno)
    local self = setmetatable({}, Zvíře)
    self.jméno = jméno
    return self
end

-- Potomek
local Pes = setmetatable({}, {__index = Zvíře})
Pes.__index = Pes

function Pes.new(jméno, rasa)
    local self = setmetatable(Zvíře.new(jméno), Pes)
    self.rasa = rasa
    return self
end
```

## Práce se soubory

### Čtení ze souboru
```lua
-- Otevření souboru
local soubor = io.open("test.txt", "r")

-- Čtení celého souboru
local obsah = soubor:read("*all")

-- Čtení po řádcích
for řádek in soubor:lines() do
    print(řádek)
end

-- Zavření souboru
soubor:close()
```

### Zápis do souboru
```lua
-- Otevření souboru pro zápis
local soubor = io.open("output.txt", "w")

-- Zápis textu
soubor:write("Hello World\n")

-- Zavření souboru
soubor:close()
```

## Pokročilé koncepty

### Metatabulky
```lua
local tabulka = {}
local meta = {
    __add = function(a, b)
        return a.hodnota + b.hodnota
    end,
    __tostring = function(self)
        return tostring(self.hodnota)
    end
}

setmetatable(tabulka, meta)
```

### Coroutines
```lua
local co = coroutine.create(function()
    for i = 1, 10 do
        print(i)
        coroutine.yield()
    end
end)

coroutine.resume(co)  -- vytiskne 1
coroutine.resume(co)  -- vytiskne 2
```

### Error handling
```lua
-- Try-catch ekvivalent
local status, err = pcall(function()
    -- Nebezpečný kód
    error("Něco se pokazilo")
end)

if not status then
    print("Chyba: " .. err)
end
```

## Tipy pro efektivní programování v Lua

### Optimalizace výkonu
```lua
-- Používejte lokální proměnné
local častá_hodnota = math.huge
-- je rychlejší než
_G.častá_hodnota = math.huge

-- Předalokujte tabulky pro velké datové sety
local velká_tabulka = table.create(1000)
```

### Konvence pojmenování
```lua
-- Používejte PascalCase pro třídy
local MojeTrida = {}

-- Používejte camelCase pro proměnné a funkce
local mojePromenna = 10
local function mojaFunkce() end

-- Používejte UPPERCASE pro konstanty
local MAX_HODNOTA = 100
```

### Debugging
```lua
-- Výpis call stacku
debug.traceback()

-- Získání informací o proměnných
debug.getlocal()

-- Nastavení breakpointu
debug.debug()
```



## Roblox-specifické koncepty v Lua

### Parenting (Rodičovství)

Parenting je základní koncept v Roblox, který definuje hierarchickou strukturu objektů.

#### Základní práce s parenting
```lua
-- Vytvoření nového objektu a nastavení rodiče
local novýObjekt = Instance.new("Part")
novýObjekt.Parent = workspace

-- Změna rodiče
novýObjekt.Parent = game.ServerStorage

-- Odstranění z hierarchie
novýObjekt.Parent = nil

-- Získání rodiče
local rodič = novýObjekt.Parent

-- Získání všech potomků
local potomci = workspace:GetChildren()
```

#### Hledání objektů v hierarchii
```lua
-- Hledání přímého potomka
local část = workspace:FindFirstChild("MojeČást")

-- Rekurzivní hledání
local hlubokýObjekt = workspace:FindFirstChild("HlubokýObjekt", true)

-- Hledání podle třídy
local světlo = workspace:FindFirstChildOfClass("PointLight")

-- Čekání na objekt
local charakter = workspace:WaitForChild("Hráč")
```

### Instance Management

#### Vytváření a manipulace s instancemi
```lua
-- Vytvoření nové instance
local část = Instance.new("Part")
část.Name = "MojeČást"
část.Position = Vector3.new(0, 10, 0)
část.Size = Vector3.new(4, 1, 2)
část.Anchored = true
část.Parent = workspace

-- Klonování instance
local kopie = část:Clone()
kopie.Parent = workspace

-- Kontrola typu instance
if část:IsA("BasePart") then
    print("Toto je základní část")
end
```

### Properties a Attributes

#### Práce s vlastnostmi
```lua
-- Nastavení vlastností
část.Transparency = 0.5
část.CanCollide = false
část.Material = Enum.Material.Neon

-- Čtení vlastností
local pozice = část.Position
local rotace = část.Orientation

-- Sledování změn vlastností
část:GetPropertyChangedSignal("Position"):Connect(function()
    print("Pozice se změnila na:", část.Position)
end)
```

#### Práce s atributy
```lua
-- Nastavení atributu
část:SetAttribute("Zdraví", 100)
část:SetAttribute("JménoHráče", "Player1")

-- Získání atributu
local zdraví = část:GetAttribute("Zdraví")

-- Sledování změn atributu
část:GetAttributeChangedSignal("Zdraví"):Connect(function()
    print("Zdraví se změnilo na:", část:GetAttribute("Zdraví"))
end)
```

### Services

#### Práce se službami
```lua
-- Získání služeb
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local ServerScriptService = game:GetService("ServerScriptService")

-- Příklad použití služeb
Players.PlayerAdded:Connect(function(player)
    print(player.Name .. " se připojil do hry")
end)
```

### Events (Události)

#### Vytváření a správa událostí
```lua
-- Vytvoření RemoteEvent
local RemoteEvents = game:GetService("ReplicatedStorage")
local mojeUdálost = Instance.new("RemoteEvent")
mojeUdálost.Name = "MojeUdálost"
mojeUdálost.Parent = RemoteEvents

-- Server-side handling
mojeUdálost.OnServerEvent:Connect(function(player, data)
    print(player.Name .. " poslal data:", data)
end)

-- Client-side firing
mojeUdálost:FireServer("Hello from client!")

-- Vytvoření BindableEvent (lokální události)
local bindableEvent = Instance.new("BindableEvent")
bindableEvent.Event:Connect(function(data)
    print("Přijata data:", data)
end)
```

### Workspace Management

#### Práce s workspace
```lua
-- Získání všech částí ve workspace
local všechnyČásti = workspace:GetDescendants()
for _, část in ipairs(všechnyČásti) do
    if část:IsA("BasePart") then
        -- Manipulace s částí
    end
end

-- Filtrování objektů
local function najdiČástiSVlastností(vlastnost, hodnota)
    local nalezené = {}
    for _, obj in ipairs(workspace:GetDescendants()) do
        if obj:IsA("BasePart") and obj[vlastnost] == hodnota then
            table.insert(nalezené, obj)
        end
    end
    return nalezené
end
```

### Model Management

#### Práce s modely
```lua
-- Vytvoření nového modelu
local model = Instance.new("Model")
model.Name = "MůjModel"
model.Parent = workspace

-- Přidání částí do modelu
local část1 = Instance.new("Part")
část1.Parent = model
local část2 = Instance.new("Part")
část2.Parent = model

-- Nastavení primární části modelu
model.PrimaryPart = část1

-- Pohyb celého modelu
model:MoveTo(Vector3.new(10, 0, 10))

-- Získání středu modelu
local střed = model:GetExtentsSize()
```

### Script Contexts

#### ServerScriptService
```lua
-- Server-side skript
local function onPlayerJoin(player)
    -- Vytvoření dat hráče
    local stats = Instance.new("Folder")
    stats.Name = "PlayerStats"
    stats.Parent = player
    
    -- Inicializace statistik
    local zdraví = Instance.new("NumberValue")
    zdraví.Name = "Health"
    zdraví.Value = 100
    zdraví.Parent = stats
end

game.Players.PlayerAdded:Connect(onPlayerJoin)
```

#### LocalScript (Client-side)
```lua
local Players = game:GetService("Players")
local lokálníHráč = Players.LocalPlayer

-- Ovládání kamery
local function upravKameru()
    local camera = workspace.CurrentCamera
    camera.FieldOfView = 70
end

-- UI prvky
local function vytvořUI()
    local screenGui = Instance.new("ScreenGui")
    screenGui.Parent = lokálníHráč.PlayerGui
    
    local tlačítko = Instance.new("TextButton")
    tlačítko.Parent = screenGui
    tlačítko.Position = UDim2.new(0.5, 0, 0.5, 0)
end
```

### Fyzika a CFrame

#### Práce s fyzikou
```lua
-- Nastavení fyzikálních vlastností
local část = Instance.new("Part")
část.Size = Vector3.new(4, 1, 2)
část.Position = Vector3.new(0, 10, 0)
část.Anchored = false
část.CanCollide = true
část.Parent = workspace

-- Přidání síly
local bodySíla = Instance.new("BodyForce")
bodySíla.Force = Vector3.new(0, 1000, 0)
bodySíla.Parent = část

-- Práce s CFrame
část.CFrame = CFrame.new(0, 10, 0) * CFrame.Angles(0, math.rad(45), 0)
```

### Optimalizace pro Roblox

#### Best Practices
```lua
-- Cachování často používaných služeb
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")

-- Efektivní aktualizace pozic
RunService.Heartbeat:Connect(function(deltaTime)
    -- Použití deltaTime pro plynulý pohyb
    část.Position = část.Position + Vector3.new(0, deltaTime * 10, 0)
end)

-- Správné použití wait()
local function optimalizovanéČekání()
    RunService.Heartbeat:Wait() -- Lepší než wait()
end
```

### Debugging v Roblox

#### Nástroje pro debugging
```lua
-- Výpis do výstupního okna
print("Běžný výpis")
warn("Varování")
error("Chybové hlášení")

-- Sledování hodnot
local function sledujHodnotu(objekt, vlastnost)
    objekt:GetPropertyChangedSignal(vlastnost):Connect(function()
        print(vlastnost, "se změnila na:", objekt[vlastnost])
    end)
end

-- Debug raycast
local function vizualizujRaycast(origin, direction)
    local ray = Instance.new("Part")
    ray.Size = Vector3.new(0.1, 0.1, direction.Magnitude)
    ray.CFrame = CFrame.new(origin + direction/2, origin + direction)
    ray.Anchored = true
    ray.CanCollide = false
    ray.Parent = workspace
    game:GetService("Debris"):AddItem(ray, 1)
end
```

