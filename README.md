--[[
    Clicker Simulator X Hub - SCRIPT OFUSCADO
    Todas as variáveis e funções foram renomeadas e strings fragmentadas.
    Funcionalidades: Auto Farm, Teleportes e Ferramentas Externas.
--]]

-- Verifica se o Hub já existe e o destrói para evitar duplicatas
if game.CoreGui:FindFirstChild("ClickerHub") then
    game.CoreGui.ClickerHub:Destroy()
end

-- Serviços do Roblox renomeados
local D = game:GetService("Players") -- Players
local V = game:GetService("VirtualUser") -- VirtualUser
local F = game:GetService("RunService") -- RunService
local G = game:GetService("TweenService") -- TweenService
local E = D.LocalPlayer -- LocalPlayer

-- Caminho da Pasta Zones para teleportes de Máquina
local H = workspace:FindFirstChild("Gameplay")
    and workspace.Gameplay:FindFirstChild("Static")
    and workspace.Gameplay.Static:FindFirstChild("Zones")

-- ==============================================================================
-- 1. CRIAÇÃO DA GUI PRINCIPAL (A, B, C)
-- ==============================================================================

local A = Instance.new("ScreenGui") -- HubGui
A.Name = "ClickerHub"
A.IgnoreGuiInset = true
A.Parent = game.CoreGui

-- Frame Principal (B: Main Frame)
local B = Instance.new("Frame")
B.Size = UDim2.new(0, 560, 0, 340)
B.Position = UDim2.new(0.5, -280, 0.5, -170)
B.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
B.Active = true 
B.Draggable = true 
B.Visible = false 
B.Parent = A
Instance.new("UICorner", B).CornerRadius = UDim.new(0, 24)

-- Botão de Abrir/Fechar (C: Open Button)
local C = Instance.new("TextButton")
C.Size = UDim2.new(0, 54, 0, 54)
C.Position = UDim2.new(0, 24, 0, 100)
C.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
C.Text = "CSX"
C.TextSize = 18
C.Font = Enum.Font.GothamBold
C.TextColor3 = Color3.fromRGB(0, 150, 255)
C.AutoButtonColor = false
C.Parent = A
Instance.new("UICorner", C).CornerRadius = UDim.new(1, 0)
Instance.new("UIStroke", C).Thickness = 2
C.Active = true
C.Draggable = true

-- LÓGICA DE ABRIR/FECHAR O HUB
C.MouseButton1Click:Connect(function()
    B.Visible = not B.Visible
end)

-- Borda animada para estética (R: AnimatedBorder, T: Time)
local R = Instance.new("UIStroke")
R.Thickness = 2
R.Color = Color3.fromRGB(255, 255, 255)
R.Transparency = 0.3
R.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
R.Parent = B

local T = 0
F.RenderStepped:Connect(function(dt)
    T += dt * 1.5
    local S = (math.sin(T) + 1) / 2
    R.Color = Color3.fromRGB(
        255 * S,
        255 * S,
        255 * S
    )
    R.Transparency = 0.15 + 0.25 * (1 - S)
end)

-- Título
local T_F = Instance.new("Frame")
T_F.Size = UDim2.new(1, -20, 0, 30)
T_F.Position = UDim2.new(0, 10, 0, 8)
T_F.BackgroundTransparency = 1
T_F.Parent = B

local T_L = Instance.new("TextLabel")
T_L.Size = UDim2.new(1, -35, 1, 0)
T_L.Position = UDim2.new(0, 35, 0, 0)
T_L.BackgroundTransparency = 1
T_L.Font = Enum.Font.GothamBold
T_L.Text = "Clicker " .. "Simulador X"
T_L.TextSize = 22
T_L.TextColor3 = Color3.fromRGB(255, 255, 255)
T_L.TextXAlignment = Enum.TextXAlignment.Left
T_L.Parent = T_F

-- Data de Atualização e Criador
local U_L = Instance.new("TextLabel")
U_L.Size = UDim2.new(0, 120, 0, 18) 
U_L.Position = UDim2.new(1, -160, 0, 6) 
U_L.BackgroundTransparency = 1
U_L.Font = Enum.Font.Gotham
U_L.Text = "Atualizado " .. "5/12/2025 (v5.1)" 
U_L.TextSize = 14 
U_L.TextColor3 = Color3.fromRGB(150, 150, 150) 
U_L.TextXAlignment = Enum.TextXAlignment.Right
U_L.Parent = B

local C_L = Instance.new("TextLabel")
C_L.Size = UDim2.new(0, 120, 0, 18) 
C_L.Position = UDim2.new(1, -160, 0, 21) 
C_L.BackgroundTransparency = 1
C_L.Font = Enum.Font.Gotham
C_L.Text = "Criador " .. "byfelipe" 
C_L.TextSize = 12 
C_L.TextColor3 = Color3.fromRGB(100, 100, 100) 
C_L.TextXAlignment = Enum.TextXAlignment.Right
C_L.Parent = B

-- Bloqueio de Posição (L_B: Lock Button)
local I_L = false -- IsLocked
local L_B = Instance.new("TextButton")
local L_C = Color3.fromRGB(40, 40, 40) -- LockColor
L_B.Size = UDim2.new(0, 28, 0, 18) 
L_B.Position = UDim2.new(0, 250, 0, 10) 
L_B.BackgroundColor3 = L_C
L_B.Text = "🔓" 
L_B.TextSize = 18
L_B.Font = Enum.Font.Gotham
L_B.TextColor3 = Color3.fromRGB(255, 255, 255)
L_B.AutoButtonColor = false
L_B.Parent = B
Instance.new("UICorner", L_B).CornerRadius = UDim.new(0, 5)

L_B.MouseButton1Click:Connect(function()
    I_L = not I_L
    
    if I_L then
        B.Active = true 
        B.Draggable = false 
        L_B.Text = "🔒" 
        L_B.BackgroundColor3 = L_C 
        print("Hub B" .. "loqueado: P" .. "osição fixa. Botões INT" .. "ERATIVOS.")
    else
        B.Active = true
        B.Draggable = true
        L_B.Text = "🔓" 
        L_B.BackgroundColor3 = L_C 
        print("Hub Desbloqu" .. "eado: Pode ser movido e inter" .. "agido.")
    end
end)

-- ==============================================================================
-- 2. TABS E ÁREAS DE CONTEÚDO
-- ==============================================================================

local S_F = Instance.new("Frame") -- Separator
S_F.Size = UDim2.new(1, -20, 0, 2)
S_F.Position = UDim2.new(0, 10, 0, 38)
S_F.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
S_F.BorderSizePixel = 0
S_F.Parent = B

local T_H = Instance.new("Frame") -- TabHolder
T_H.Size = UDim2.new(0, 545, 0, 32) 
T_H.Position = UDim2.new(0, 10, 0, 48)
T_H.BackgroundTransparency = 1
T_H.Parent = B

local T_B_S = UDim2.new(0, 100, 0, 28) -- TabButtonSize

-- Função para criar Botões de Aba (I: CreateTabButton)
local function I(name, icon, xOffset)
    local Button = Instance.new("TextButton")
    Button.Name = name .. "Tab"
    Button.Size = T_B_S
    Button.Position = UDim2.new(0, xOffset, 0, 0)
    Button.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    Button.BorderSizePixel = 0
    Button.AutoButtonColor = false
    Button.Font = Enum.Font.GothamSemibold
    Button.Text = icon .. " " .. name 
    Button.TextSize = 16
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.Parent = T_H
    Instance.new("UICorner", Button).CornerRadius = UDim.new(0, 10)
    return Button
end

-- Configurações de Tween para Animação de Clique (T_C_I: TweenClickInfo, S_F: ScaleFactor)
local T_C_I = TweenInfo.new(0.08, Enum.EasingStyle.Sine, Enum.EasingDirection.Out)
local S_F = 0.95 

-- Criação dos Botões de Aba
local T1 = I("Main", "🏠", 0) -- MainTab
local T2 = I("Teleport", "🗺️", 110) -- TeleportTab
local T3 = I("Machines", "⚙️", 220) -- MachinesTab
local T4 = I("Eggs", "🥚", 330) -- EggsTab      
local T5 = I("Settings", "🛠️", 440) -- SettingsTab

local L_S = Instance.new("Frame") -- LineSeparator
L_S.Size = UDim2.new(1, -20, 0, 1)
L_S.Position = UDim2.new(0, 10, 0, 84)
L_S.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
L_S.BorderSizePixel = 0
L_S.Parent = B

-- Função para criar Áreas de Conteúdo (J: CreateContentArea)
local function J(initialCanvasY)
    local Area = Instance.new("ScrollingFrame")
    Area.Size = UDim2.new(1, -40, 1, -96)
    Area.Position = UDim2.new(0, 20, 0, 90)
    Area.BackgroundTransparency = 1
    Area.Visible = false
    Area.Parent = B
    Area.ScrollBarThickness = 6
    Area.CanvasSize = UDim2.new(0, 0, 0, initialCanvasY or 300) 
    Area.Active = true
    return Area
end

local A1 = J(350) -- MainArea
local A2 = J(600) -- TeleportArea
local A3 = J(300) -- MachinesArea
local A4 = J(300) -- EggsArea
local A5 = J(550) -- SettingsArea

local T_B_L = { -- TabButtonList
    Main      = T1,
    Teleport  = T2,
    Machines  = T3,
    Eggs      = T4,      
    Settings  = T5
}

local C_A_L = { -- ContentAreaList
    Main      = A1,
    Teleport  = A2, 
    Machines  = A3,
    Eggs      = A4,     
    Settings  = A5 
}

-- Lógica de clique nas abas
for t_N, T_B in pairs(T_B_L) do
    local O_S = T_B.Size -- OriginalSize
    local C_S = UDim2.new(0, O_S.X.Offset * S_F, 0, O_S.Y.Offset * S_F) -- ClickedSize
    
    T_B.MouseButton1Click:Connect(function()
        
        -- 1. Animação de clique
        G:Create(T_B, T_C_I, {Size = C_S}):Play()

        task.wait(0.08)

        -- 2. Animação de retorno
        G:Create(T_B, T_C_I, {Size = O_S}):Play()

        -- 3. Troca de abas (Esconde todas e mostra a ativa)
        for _, area in pairs(C_A_L) do
            area.Visible = false
        end
        for _, button in pairs(T_B_L) do
            button.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
        end
        
        if C_A_L[t_N] then
            C_A_L[t_N].Visible = true
        end
        T_B.BackgroundColor3 = Color3.fromRGB(50, 50, 50) -- Cor de Aba Ativa
    end)
end

-- Garante que o Main seja a aba inicial
T1.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
A1.Visible = true

-- ==============================================================================
-- 3. FUNÇÕES DE CONTROLE (K: Toggle, L: Button)
-- ==============================================================================

-- Cria um Botão de Toggle (K: CreateToggleButton)
local function K(parentFrame, labelText, yPosition, callbackFunction)
    local T_F = Instance.new("Frame")
    T_F.Size = UDim2.new(0, 307, 0, 44)
    T_F.Position = UDim2.new(0, 0, 0, yPosition)
    T_F.BackgroundColor3 = Color3.fromRGB(31, 31, 31)
    T_F.BorderSizePixel = 0
    T_F.Parent = parentFrame
    Instance.new("UICorner", T_F).CornerRadius = UDim.new(0, 16)

    local L = Instance.new("TextLabel")
    L.Size = UDim2.new(0.7, 0, 1, 0)
    L.Position = UDim2.new(0, 14, 0, 0)
    L.BackgroundTransparency = 1
    L.Font = Enum.Font.Gotham
    L.Text = labelText
    L.TextSize = 18
    L.TextXAlignment = Enum.TextXAlignment.Left
    L.TextColor3 = Color3.fromRGB(255, 255, 255)
    L.Parent = T_F

    local B = Instance.new("TextButton")
    B.Size = UDim2.new(0, 48, 0, 22)
    B.Position = UDim2.new(1, -70, 0.5, -11)
    B.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    B.AutoButtonColor = false
    B.Text = ""
    B.Parent = T_F 
    Instance.new("UICorner", B).CornerRadius = UDim.new(1, 0)

    local S = Instance.new("Frame")
    S.Size = UDim2.new(0, 18, 0, 18)
    S.Position = UDim2.new(0, 2, 0.5, -9)
    S.BackgroundColor3 = Color3.fromRGB(200, 30, 30) -- Cor Inicial (Vermelho OFF)
    S.Parent = B
    Instance.new("UICorner", S).CornerRadius = UDim.new(1, 0)
    
    local I_E = false -- IsEnabled
    local T_T = 0.15 -- TweenTime

    local function S_T_S(state) -- SetToggleState
        I_E = state
        local n_C, n_P -- newColor, newPosition
        
        if state then
            n_C = Color3.fromRGB(50, 170, 50)
            n_P = UDim2.new(1, -20, 0.5, -9)
            G:Create(S, TweenInfo.new(T_T), {BackgroundColor3 = Color3.fromRGB(50, 220, 70)}):Play()
        else
            n_C = Color3.fromRGB(60, 60, 60)
            n_P = UDim2.new(0, 2, 0.5, -9)
            G:Create(S, TweenInfo.new(T_T), {BackgroundColor3 = Color3.fromRGB(200, 30, 30)}):Play()
        end

        G:Create(B, TweenInfo.new(T_T), {BackgroundColor3 = n_C}):Play()
        G:Create(S, TweenInfo.new(T_T), {Position = n_P}):Play()
        
        callbackFunction(state)
    end
    
    S_T_S(false)

    B.MouseButton1Click:Connect(function()
        S_T_S(not I_E)
    end)
    return T_F
end

-- Cria um Botão Simples Estilizado (L: CreateStyledButton)
local function L(parentFrame, labelText, yPosition, callbackFunction, buttonColor)
    local D_C = Color3.fromRGB(31, 31, 31) -- DefaultColor
    
    local B = Instance.new("TextButton")
    B.Size = UDim2.new(0, 307, 0, 44)
    B.Position = UDim2.new(0, 0, 0, yPosition) 
    B.BackgroundColor3 = buttonColor or D_C 
    B.BorderSizePixel = 0
    B.AutoButtonColor = true 
    B.Font = Enum.Font.Gotham
    B.Text = labelText
    B.TextSize = 18
    B.TextColor3 = Color3.fromRGB(255, 255, 255) 
    B.Parent = parentFrame
    Instance.new("UICorner", B).CornerRadius = UDim.new(0, 16)
    
    B.MouseButton1Click:Connect(function()
        callbackFunction() 
    end)
    return B
end

-- ==============================================================================
-- 4. LÓGICAS DE FUNCIONALIDADES
-- ==============================================================================

local A_A_C -- AntiAfkConnection
local A_B_F_T -- AutoBossFarmThread
local S_C_F -- SavedCFrame
local I_B_F = false -- IsBossFarming
local I_F = false -- IsFlying
local F_C -- FlyConnection

-- Lógica para o Anti AFK (M: AntiAFK_Callback)
local function M(state)
    if A_A_C then A_A_C:Disconnect(); A_A_C = nil end
    if state then 
        A_A_C = E.Idled:Connect(function()
            V:CaptureController()
            V:ClickButton2(Vector2.new()) 
            print("Anti " .. "AFK: Simulação de clique execut" .. "ada.")
        end)
    end
end

-- Função auxiliar para encontrar Hitboxes de Boss (G_A_B_P)
local function G_A_B_P()
    local B_F = workspace:FindFirstChild("Gameplay")
        and workspace.Gameplay:FindFirstChild("Dynamic")
        and workspace.Gameplay.Dynamic:FindFirstChild("Bosses")
    if not B_F then return {} end

    local T_P = {} -- TargetParts
    for _, obj in ipairs(B_F:GetChildren()) do
        if obj:IsA("BasePart") and (string.find(obj.Name, "Combo_", 1, true) or string.find(obj.Name, "Hitbox", 1, true)) then
            table.insert(T_P, obj)
        end
        if obj:IsA("Model") then
            local H_B = obj:FindFirstChild("Hitbox")
            if H_B and H_B:IsA("BasePart") then table.insert(T_P, H_B) end
        end
    end
    return T_P
end

-- Lógica para o Auto Farm Boss (N: AutoFarmBoss_Callback)
local function N(state)
    I_B_F = state
    
    if A_B_F_T then
        task.cancel(A_B_F_T)
        A_B_F_T = nil
    end

    local C = E.Character or E.CharacterAdded:Wait()
    local R_P = C:FindFirstChild("HumanoidRootPart") -- RootPart
    
    if not state then
        if R_P and S_C_F then
            R_P.CFrame = S_C_F
        end
        return
    end

    if R_P then
        S_C_F = R_P.CFrame
    end

    A_B_F_T = task.spawn(function()
        while I_B_F and R_P do
            
            local B_P = G_A_B_P() -- BossParts
            
            if #B_P > 0 then
                local T_P = B_P[1] -- TargetPart
                
                if T_P and T_P.Parent then
                    -- Calcula uma posição ligeiramente afastada do hitbox
                    local O = T_P.CFrame.LookVector * -5 -- Offset
                    R_P.CFrame = T_P.CFrame + Vector3.new(0, 1.5, 0) + O
                    
                    -- Fica na posição enquanto o boss estiver ativo
                    while I_B_F and T_P.Parent do
                        task.wait(0.1) 
                    end
                end

                -- Volta para a posição salva após o boss morrer/desaparecer
                if R_P and S_C_F then
                    R_P.CFrame = S_C_F
                end
            end
            
            task.wait(1) 
        end
        
        -- Garante que o jogador retorne à posição original ao desativar
        if R_P and S_C_F then
            R_P.CFrame = S_C_F
        end
    end)
end

-- Lógica para Teleporte (Ilhas) (O: TeleportToIsland)
local function O(islandFolderName)
    local I_F = workspace:FindFirstChild("Islands") or workspace:FindFirstChild("island") -- IslandsFolder
    if not I_F then warn("Pasta principal " .. "'Islands' ou 'island' não encontrada no Workspace."); return end
    
    local I_M = I_F:FindFirstChild(islandFolderName) -- IslandModel
    
    -- Tenta encontrar a ilha com um nome alternativo
    if not I_M then
        local A_N = string.gsub(islandFolderName, "zone", "_island") -- alternativeName
        I_M = I_F:FindFirstChild(A_N)
        if not I_M then warn("Tentativas de " .. "encontrar a Ilha '" .. islandFolderName .. "' falharam."); return end
    end

    local T_T = I_M:FindFirstChild("Origin") or I_M:FindFirstChild("origin") -- TeleportTarget
    if not T_T then T_T = I_M:FindFirstChildOfClass("BasePart") end
    if not T_T and I_M:IsA("Model") and I_M.PrimaryPart then T_T = I_M.PrimaryPart end

    local C = E.Character
    local R_P = C and C:FindFirstChild("HumanoidRootPart")

    if R_P and T_T and T_T:IsA("BasePart") then
        R_P.CFrame = T_T.CFrame + Vector3.new(0, 5, 0)
        print("Teleporte para: " .. I_M.Name .. " executado com sucesso.")
    else
        warn("Ponto de teleporte " .. "para '" .. I_M.Name .. "' não é válido ou não foi encontrado.")
    end
end

-- Lógica para Teleporte (Spawn Inicial) (T_T_S: TeleportToSpawn)
local function T_T_S()
    local S_T = workspace:FindFirstChild("SpawnLocation") -- SpawnTarget
        or workspace:FindFirstChild("Spawn") 
        or workspace:FindFirstChildOfClass("SpawnPoint")

    local C = E.Character
    local R_P = C and C:FindFirstChild("HumanoidRootPart")

    if R_P and S_T and S_T:IsA("BasePart") then
        R_P.CFrame = S_T.CFrame + Vector3.new(0, 5, 0)
        print("Teleporte para Spawn " .. "executado com sucesso.")
    else
        warn("Ponto de teleporte " .. "para Spawn não encontrado ou não é válido.")
    end
end

-- Lógica para Fly PC e Mobile (T_F: ToggleFly)
local function T_F()
    I_F = not I_F
    
    local C = E.Character
    local R_P = C and C:FindFirstChild("HumanoidRootPart")
    local H = C and C:FindFirstChildOfClass("Humanoid")

    if not R_P or not H then 
        warn("Personagem ou " .. "HumanoidRootPart não encontrados.")
        I_F = false 
        return 
    end

    if I_F then
        H.PlatformStand = true 
        R_P.Anchored = true
        R_P.CanCollide = false
        print("Fly Ativado. Use as teclas " .. "de movimento para 'voar'.")
        
        F_C = F.RenderStepped:Connect(function()
            -- Determina a direção do movimento baseada na entrada do usuário
            local M_V = Vector3.new( -- MoveVector
                E.Humanoid.MoveDirection.X,
                E.Humanoid.MoveDirection.Y,
                E.Humanoid.MoveDirection.Z
            ) * 1.5 
            
            -- Move o jogador
            R_P.CFrame = R_P.CFrame * CFrame.new(M_V)
        end)
    else
        if F_C then F_C:Disconnect() end
        H.PlatformStand = false
        R_P.Anchored = false
        R_P.CanCollide = true
        print("Fly Desativado.")
    end
end

-- Lógica de injeção do Infinity Yield (I_I_Y: InjectInfinityYield)
local function I_I_Y()
    local S, E_R = pcall(function() -- Success, Error
        loadstring(game:HttpGet("https://raw.githubusercontent.com/EdgeIY/infinite-yield/master/source"))()
    end)
    
    if S then
        print("Infinity Y" .. "ield Injetado com Su" .. "cesso! Procure por sua interface na tela.")
    else
        warn("Falha ao Injetar I" .. "nfinity Yield: " .. tostring(E_R) .. ". O módulo pode não estar disponível ou o executor não suporta httpget/loadstring.")
    end
end

-- Lógica de injeção do Dex Explorer V2 (I_D_E: InjectDexExplorerV2)
local function I_D_E()
    local S, E_R = pcall(function()
        loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Dex-v2-29862"))()
    end)
    
    if S then
        print("Dex Expl" .. "orer V2 Injetado com Su" .. "cesso! Procure por sua interface na tela.")
    else
        warn("Falha ao Injetar D" .. "ex Explorer V2: " .. tostring(E_R) .. ". Verifique se o executor suporta httpget/loadstring.")
    end
end

-- Lógica de injeção do Remote Spy (I_R_S: InjectRemoteSpy)
local function I_R_S()
    local S, E_R = pcall(function()
        loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-remote-spy-mobile-reupload-24466"))()
    end)
    
    if S then
        print("Remote " .. "Spy Injetado com Su" .. "cesso! Procure por sua interface na tela.")
    else
        warn("Falha ao Injetar R" .. "emote Spy: " .. tostring(E_R) .. ". Verifique se o executor suporta httpget/loadstring.")
    end
end


-- Lógica de Teleporte (Ovos de Restock) (P: TeleportToIndexedObject)
local function P(objectName, parentContainerName, objectIndex) 
    print("--- Tentativa " .. "de Teleporte Indexado ---")
    
    local C_N = nil -- Container
    local F_O = {} -- FoundObjects
    
    -- 1. ENCONTRAR O CONTÊINER (com tratamento especial para RestockingShops)
    if parentContainerName == "Restock" .. "ingShops" then
        local G_P = workspace:FindFirstChild("Gameplay") -- Gameplay
        local S_P = G_P and G_P:FindFirstChild("Static") -- Static
        local R_S = S_P and S_P:FindFirstChild("Restock" .. "ingShops") -- RestockingShops
        
        if R_S then
            C_N = R_S
        else
            warn("STATUS 1/3: FALHA ao enco" .. "ntrar 'RestockingShops'.")
            return
        end
    else
        C_N = workspace:FindFirstChild(parentContainerName, true)
        if not C_N then
            warn("Contêiner '"..parentContainerName.."' não encontrado. Retornando.")
            return
        end
    end
    
    -- 2. ENCONTRAR TODAS AS INSTÂNCIAS COM O NOME CORRETO
    for _, child in ipairs(C_N:GetChildren()) do
        if child.Name == objectName then
            table.insert(F_O, child)
        end
    end
    
    if #F_O < objectIndex then
        warn("STATUS 2/3: FALHA! Objeto no índice " .. objectIndex .. " NÃO enco" .. "ntrado (Total de objetos: " .. #F_O .. ").")
        return
    end

    local T_O = F_O[objectIndex] -- TargetObject
    
    local C_P = E.Character -- Character
    local R_P = C_P and C_P:FindFirstChild("HumanoidRootPart") -- RootPart

    if R_P and T_O then
        local T_C_F = nil -- TargetCFrame
        
        -- 3. DETERMINAR O CFRAME CORRETO
        if T_O:IsA("Model") then
            if T_O.PrimaryPart then
                T_C_F = T_O.PrimaryPart.CFrame
            else
                local S_P, P_V = pcall(T_O.GetPivot, T_O) -- Success, Pivot
                if S_P then T_C_F = P_V end
            end
        elseif T_O:IsA("BasePart") then
            T_C_F = T_O.CFrame
        end

        if T_C_F and typeof(T_C_F) == "CFrame" then
            R_P.CFrame = T_C_F + Vector3.new(0, 5, 0)
            print("Teleporte para: " .. T_O.Name .. " em '" .. C_N.Name .. "' EX" .. "ECUTADO com sucesso.")
        else
            warn("Teleporte cancelado: Pivot/CFram" .. "e não é válido ou não foi encontrado.")
        end
    else
        warn("Teleporte cancelado: Pers" .. "onagem (HumanoidRootPart) não encontrado.")
    end
end

-- ==============================================================================
-- 5. POPULAÇÃO DAS ABAS (Q, Pop_M, Pop_E, Pop_S)
-- ==============================================================================

-- Popula a aba Teleport (Q: PopulateTeleportArea)
local function Q()
    for _, child in pairs(A2:GetChildren()) do child:Destroy() end

    local Y_P = 6 -- YPosition
    local B_S = 50 -- ButtonSpacing
    
    -- 1. SEÇÃO SPAWN
    local S_L = Instance.new("TextLabel") -- SpawnLabel
    S_L.Name = "SpawnLabel"
    S_L.Size = UDim2.new(0, 260, 0, 22); S_L.Position = UDim2.new(0, 0, 0, Y_P)
    S_L.BackgroundTransparency = 1; S_L.Font = Enum.Font.GothamSemibold
    S_L.Text = "Spawn"; S_L.TextSize = 18
    S_L.TextXAlignment = Enum.TextXAlignment.Left; S_L.TextColor3 = Color3.fromRGB(220, 220, 220)
    S_L.Parent = A2
    Y_P += 24 

    L(A2, "Teleport Início", Y_P, T_T_S)
    Y_P += B_S 

    -- 2. SEÇÃO ILHAS
    local I_L = Instance.new("TextLabel") -- IslandsLabel
    I_L.Name = "IslandLabel"
    I_L.Size = UDim2.new(0, 260, 0, 22); I_L.Position = UDim2.new(0, 0, 0, Y_P)
    I_L.BackgroundTransparency = 1; I_L.Font = Enum.Font.GothamSemibold
    I_L.Text = "Teleporte " .. "de Ilhas"; I_L.TextSize = 18
    I_L.TextXAlignment = Enum.TextXAlignment.Left; I_L.TextColor3 = Color3.fromRGB(220, 220, 220)
    I_L.Parent = A2
    Y_P += 24
    
    local I_O = { -- IslandOptions
        {"Nature Island", "naturezone"}, {"Desert Island", "desertzone"},  
        {"Volcano Island", "volcanozone"}, {"Water Island", "waterzone"},
        {"Ice Island", "icezone"}, {"Cloud Island", "cloudzone"},
        {"Cosmic Island", "cosmic_island"},  {"Boss Island", "bosszone"}, 
    }

    for _, I_D in ipairs(I_O) do -- IslandData
        L(A2, I_D[1], Y_P, function()
            O(I_D[2])
        end)
        Y_P += B_S
    end

    A2.CanvasSize = UDim2.new(0, 0, 0, math.max(A2.AbsoluteSize.Y, Y_P + 10))
end

-- Popula a aba Machines (Pop_M)
local function Pop_M()
    for _, child in pairs(A3:GetChildren()) do child:Destroy() end

    local M_L = Instance.new("TextLabel") -- MachineLabel
    M_L.Name = "MachineLabel"
    M_L.Size = UDim2.new(0, 260, 0, 22); M_L.Position = UDim2.new(0, 0, 0, 6)
    M_L.BackgroundTransparency = 1; M_L.Font = Enum.Font.GothamSemibold
    M_L.Text = "Máquinas e " .. "Criação"; M_L.TextSize = 18
    M_L.TextXAlignment = Enum.TextXAlignment.Left; M_L.TextColor3 = Color3.fromRGB(220, 220, 220)
    M_L.Parent = A3

    local Y_P = 30
    local B_S = 50 
    
    local function C_M_B(machineName, displayName, yPos) -- CreateMachineButton
        local function T_T_M() -- TeleportToMachine
            local T_O = H and H:FindFirstChild(machineName) -- TargetObject

            local T_P = T_O -- TargetPart
            if T_O and (T_O:IsA("Model") or T_O:IsA("Folder")) then
                -- Tenta encontrar a parte principal para teleporte
                T_P = T_O:FindFirstChildOfClass("BasePart")
            end
            
            local C = E.Character
            local R_P = C and C:FindFirstChild("HumanoidRootPart")

            if T_P and R_P and T_P:IsA("BasePart") then
                R_P.CFrame = T_P.CFrame + Vector3.new(0, 5, 0)
                print("Teleporte para: " .. displayName .. " executado com sucesso.")
            else
                print("Objeto da máquina '" .. machineName .. "' não encontrado ou não é uma BasePart válida para teleporte.")
            end
        end
        L(A3, displayName, yPos, T_T_M)
    end
    
    C_M_B("GoldMachine", "Gold Machine", Y_P); Y_P += B_S
    C_M_B("RainbowMachine", "Rainbow Machine", Y_P); Y_P += B_S
    C_M_B("CosmicMachine", "Cosmic Machine", Y_P); Y_P += B_S
    
    -- NOVO BOTÃO ADICIONADO AQUI
    C_M_B("Forge", "Forge", Y_P); Y_P += B_S
    
    C_M_B("Daycare", "Daycare", Y_P); Y_P += B_S
    
    A3.CanvasSize = UDim2.new(0, 0, 0, math.max(A3.AbsoluteSize.Y, Y_P + 10))
end

-- Popula a aba Eggs (Pop_E)
local function Pop_E()
    for _, child in pairs(A4:GetChildren()) do child:Destroy() end
    
    local Y_P = 6
    local B_S = 50 

    local E_L = Instance.new("TextLabel") -- EggsLabel
    E_L.Name = "EggsLabel"
    E_L.Size = UDim2.new(0, 260, 0, 22); E_L.Position = UDim2.new(0, 0, 0, Y_P)
    E_L.BackgroundTransparency = 1; E_L.Font = Enum.Font.GothamSemibold
    E_L.Text = "Teleporte " .. "de Ovos Restock";
    E_L.TextSize = 18
    E_L.TextXAlignment = Enum.TextXAlignment.Left; E_L.TextColor3 = Color3.fromRGB(220, 220, 220)
    E_L.Parent = A4
    Y_P += 24

    local D_C = Color3.fromRGB(31, 31, 31) -- DefaultButtonColor

    local function C_R_E_B(Index) -- CreateRestockEggButton
        local E_N = "Restocking" .. "_Egg" -- EggName
        local C_N = "Restock" .. "ingShops" -- ContainerName
        
        L(A4, "Egg Restocking " .. Index, Y_P, function() 
            P(E_N, C_N, Index) 
        end, D_C) 
        Y_P += B_S
    end

    C_R_E_B(1)
    C_R_E_B(2)
    C_R_E_B(3)
    C_R_E_B(4)
    
    A4.CanvasSize = UDim2.new(0, 0, 0, math.max(A4.AbsoluteSize.Y, Y_P + 10))
end

-- Popula a aba Settings (Pop_S)
local function Pop_S()
    for _, child in pairs(A5:GetChildren()) do child:Destroy() end
    
    local Y_P = 6
    local B_S = 50

    local G_L = Instance.new("TextLabel") -- GlobalLabel
    G_L.Size = UDim2.new(0, 260, 0, 22); G_L.Position = UDim2.new(0, 0, 0, Y_P)
    G_L.BackgroundTransparency = 1; G_L.Font = Enum.Font.GothamSemibold
    G_L.Text = "Configuraç" .. "ões Globais"; G_L.TextSize = 18
    G_L.TextXAlignment = Enum.TextXAlignment.Left; G_L.TextColor3 = Color3.fromRGB(220, 220, 220)
    G_L.Parent = A5
    Y_P = 30

    -- 1. Anti AFK (Toggle)
    K(A5, "Anti AFK", Y_P, M); Y_P += B_S

    -- 2. Fly (Botão de Toggle Manual)
    local D_C = Color3.fromRGB(31, 31, 31)
    L(A5, "Fly PC e Mobile", Y_P, T_F, D_C); Y_P += B_S

    -- 3. Infinity Yield (Botão de Injeção)
    L(A5, "Infinity Yield (Inject)", Y_P, I_I_Y); Y_P += B_S

    -- 4. Criadores / Exploradores (NOVA SEÇÃO)
    Y_P += 10
    local C_L_A = Instance.new("TextLabel") -- CreatorsLabel
    C_L_A.Size = UDim2.new(0, 260, 0, 22); C_L_A.Position = UDim2.new(0, 0, 0, Y_P) 
    C_L_A.BackgroundTransparency = 1; C_L_A.Font = Enum.Font.GothamSemibold
    C_L_A.Text = "Criadores / " .. "Exploradores"; C_L_A.TextSize = 18
    C_L_A.TextXAlignment = Enum.TextXAlignment.Left; C_L_A.TextColor3 = Color3.fromRGB(220, 220, 220)
    C_L_A.Parent = A5
    Y_P += 24

    -- 5. Dex Explorer V2 (NOVO BOTÃO)
    L(A5, "Dex Explorer V2", Y_P, I_D_E); Y_P += B_S

    -- 6. Remote Spy (NOVO BOTÃO)
    L(A5, "Remote Spy", Y_P, I_R_S); Y_P += B_S

    -- 7. Misc
    Y_P += 10
    local M_L_A = Instance.new("TextLabel") -- MiscLabel
    M_L_A.Size = UDim2.new(0, 260, 0, 22); M_L_A.Position = UDim2.new(0, 0, 0, Y_P) 
    M_L_A.BackgroundTransparency = 1; M_L_A.Font = Enum.Font.GothamSemibold
    M_L_A.Text = "Misc"; M_L_A.TextSize = 18
    M_L_A.TextXAlignment = Enum.TextXAlignment.Left; M_L_A.TextColor3 = Color3.fromRGB(220, 220, 220)
    M_L_A.Parent = A5
    Y_P += 24

    -- 8. Delete Hub
    local R_C = Color3.fromRGB(200, 30, 30) -- RedColor
    L(A5, "Delete Hub", Y_P, function()
        local H_D = game.CoreGui:FindFirstChild("ClickerHub") -- HubToDelete
        if H_D then
            H_D:Destroy()
            print("Hub 'Clicker" .. "Hub' excluído com sucesso.")
        end
    end, R_C); Y_P += B_S

    A5.CanvasSize = UDim2.new(0, 0, 0, math.max(A5.AbsoluteSize.Y, Y_P + 10))
end


-- ==============================================================================
-- 6. POPULAÇÃO DA ABA MAIN E CHAMADAS INICIAIS (Pop_M_A)
-- ==============================================================================

local function Pop_M_A()
    for _, child in pairs(A1:GetChildren()) do child:Destroy() end

    local Y_P = 6
    local B_S = 50

    -- 1. Seção Auto Farm
    local A_F_L = Instance.new("TextLabel") -- AutoFarmLabel
    A_F_L.Size = UDim2.new(0, 260, 0, 22); A_F_L.Position = UDim2.new(0, 0, 0, Y_P)
    A_F_L.BackgroundTransparency = 1; A_F_L.Font = Enum.Font.GothamSemibold
    A_F_L.Text = "Auto Farm"; A_F_L.TextSize = 18
    A_F_L.TextXAlignment = Enum.TextXAlignment.Left; A_F_L.TextColor3 = Color3.fromRGB(220, 220, 220)
    A_F_L.Parent = A1
    Y_P = 30

    -- Toggle: Auto Farm Boss
    K(A1, "Auto Farm Boss", Y_P, N); Y_P += B_S

    -- 2. Seção Evento 
    Y_P += 10 
    local E_L = Instance.new("TextLabel") -- EventLabel
    E_L.Size = UDim2.new(0, 260, 0, 22); E_L.Position = UDim2.new(0, 0, 0, Y_P)
    E_L.BackgroundTransparency = 1; E_L.Font = Enum.Font.GothamSemibold
    E_L.Text = "Evento"; E_L.TextSize = 18
    E_L.TextXAlignment = Enum.TextXAlignment.Left; E_L.TextColor3 = Color3.fromRGB(220, 220, 220)
    E_L.Parent = A1
    Y_P += 24

    local G_C = Color3.fromRGB(31, 31, 31) -- GrayColor
    L(A1, "🔒 Em " .. "breve", Y_P, function() 
        print("Botão 'Em breve' cli" .. "cado. Nenhuma função ativa associada.")
    end, G_C); Y_P += B_S

    A1.CanvasSize = UDim2.new(0, 0, 0, math.max(A1.AbsoluteSize.Y, Y_P + 10))
end


-- Chamadas para popular todas as abas
Pop_M_A()
Q()
Pop_M()
Pop_E() 
Pop_S()
