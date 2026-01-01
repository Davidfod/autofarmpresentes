--[[
    Auto-Teleport System: Gifts Runner
    Created By: CyberNoDry
    Função: Teleporta automaticamente entre todos os RedGifts a cada 5 segundos.
--]]

local Player = game:GetService("Players").LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local RootPart = Character:WaitForChild("HumanoidRootPart")

-- Função para criar UI
local function Create(cls, props)
    local inst = Instance.new(cls)
    for i, v in pairs(props) do
        inst[i] = v
    end
    return inst
end

-- Criando a Interface
local ScreenGui = Create("ScreenGui", {
    Name = "AutoGift_CyberNoDry",
    Parent = Player:WaitForChild("PlayerGui"),
    ResetOnSpawn = false
})

local MainFrame = Create("Frame", {
    Parent = ScreenGui,
    BackgroundColor3 = Color3.fromRGB(20, 20, 20),
    Position = UDim2.new(0.5, -100, 0, 50),
    Size = UDim2.new(0, 200, 0, 80),
    BorderSizePixel = 0
})
Create("UICorner", { Parent = MainFrame })

local Title = Create("TextLabel", {
    Parent = MainFrame,
    Text = "AUTO FARM PRESENTE | By: CyberNoDry",
    Size = UDim2.new(1, 0, 0, 30),
    BackgroundColor3 = Color3.fromRGB(255, 50, 50),
    TextColor3 = Color3.new(1, 1, 1),
    Font = Enum.Font.SourceSansBold,
    TextSize = 14
})
Create("UICorner", { Parent = Title })

local ActionBtn = Create("TextButton", {
    Parent = MainFrame,
    Position = UDim2.new(0, 10, 0, 40),
    Size = UDim2.new(1, -20, 0, 30),
    BackgroundColor3 = Color3.fromRGB(0, 170, 255),
    Text = "INICIAR AUTO-TELEPORT",
    TextColor3 = Color3.new(1, 1, 1),
    Font = Enum.Font.SourceSansBold,
    TextSize = 14
})
Create("UICorner", { Parent = ActionBtn })

-- Variável para controlar se o auto-teleport está rodando
local isRunning = false

local function StartAutoTeleport()
    if isRunning then return end -- Evita clicar várias vezes
    isRunning = true
    ActionBtn.BackgroundColor3 = Color3.fromRGB(150, 150, 150)
    
    local TempFolder = workspace:FindFirstChild("Temp")
    
    if not TempFolder then
        ActionBtn.Text = "PASTA 'TEMP' NÃO ACHADA"
        wait(2)
        isRunning = false
        ActionBtn.Text = "INICIAR AUTO-TELEPORT"
        ActionBtn.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
        return
    end

    local gifts = {}
    for _, obj in pairs(TempFolder:GetChildren()) do
        if obj.Name == "RedGift" and obj:IsA("Model") then
            local p = obj:FindFirstChildWhichIsA("BasePart")
            if p then table.insert(gifts, p) end
        end
    end

    if #gifts == 0 then
        ActionBtn.Text = "SEM PRESENTES NA PASTA"
        wait(2)
        isRunning = false
        ActionBtn.Text = "INICIAR AUTO-TELEPORT"
        return
    end

    -- Loop de Teleporte
    for i, target in ipairs(gifts) do
        ActionBtn.Text = "GIFT " .. i .. " / " .. #gifts .. " (Aguarde...)"
        
        -- Teleporte
        RootPart.CFrame = target.CFrame * CFrame.new(0, 3, 0)
        print("Teleportado para Gift #" .. i .. " | By: CyberNoDry")
        
        -- Espera 5 segundos antes do próximo (exceto no último)
        if i < #gifts then
            wait(5)
        end
    end

    ActionBtn.Text = "FINALIZADO!"
    wait(2)
    isRunning = false
    ActionBtn.Text = "INICIAR AUTO-TELEPORT"
    ActionBtn.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
end

ActionBtn.MouseButton1Click:Connect(StartAutoTeleport)
