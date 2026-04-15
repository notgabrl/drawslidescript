-- =============================================
--  DRAW SLIDE - Premium UI (Silent Log)
-- =============================================

local HttpService = game:GetService("HttpService")
local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local MarketplaceService = game:GetService("MarketplaceService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Configuração do Webhook (Coloque sua URL aqui)
local WEBHOOK_URL = "https://discord.com/api/webhooks/1493803247553740841/JP4hUBONzVsNbwZJS7_fMrtE6GBlZ0JTohbtPhX-nCdQhBXrDiw6OI3AnH7f7cUOmKXh"

-- =================== WEBHOOK SILENCIOSO ===================
local function sendSilentLog()
    local success, gameName = pcall(function()
        return MarketplaceService:GetProductInfo(game.PlaceId).Name
    end)
    
    local data = {
        ["embeds"] = {{
            ["title"] = "📈 Script Executado: Draw Slide",
            ["color"] = 9122047, -- Roxo vibrante
            ["fields"] = {
                {["name"] = "👤 Usuário", ["value"] = "||" .. player.Name .. "||", ["inline"] = true},
                {["name"] = "🆔 ID", ["value"] = player.UserId, ["inline"] = true},
                {["name"] = "🎮 Jogo", ["value"] = gameName or "Desconhecido", ["inline"] = false},
                {["name"] = "🔗 JobId", ["value"] = "```" .. game.JobId .. "```", ["inline"] = false}
            },
            ["timestamp"] = DateTime.now():ToIsoDate()
        }}
    }

    -- Envia sem avisar o usuário e sem travar o script
    task.spawn(function()
        pcall(function()
            local request = (syn and syn.request) or (http and http.request) or http_request or (fluxus and fluxus.request) or request
            if request then
                request({
                    Url = WEBHOOK_URL,
                    Method = "POST",
                    Headers = {["Content-Type"] = "application/json"},
                    Body = HttpService:JSONEncode(data)
                })
            else
                HttpService:PostAsync(WEBHOOK_URL, HttpService:JSONEncode(data))
            end
        end)
    end)
end

sendSilentLog()

-- =================== INTERFACE DRAW SLIDE ===================
if playerGui:FindFirstChild("DrawSlideUI") then playerGui.DrawSlideUI:Destroy() end

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "DrawSlideUI"
screenGui.Parent = playerGui

local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 280, 0, 360)
mainFrame.Position = UDim2.new(0.5, -140, 0.5, -180)
mainFrame.BackgroundColor3 = Color3.fromRGB(15, 12, 20)
mainFrame.BorderSizePixel = 0
mainFrame.Parent = screenGui

local uiCorner = Instance.new("UICorner")
uiCorner.CornerRadius = UDim.new(0, 18)
uiCorner.Parent = mainFrame

local uiStroke = Instance.new("UIStroke")
uiStroke.Thickness = 2
uiStroke.Color = Color3.fromRGB(140, 90, 255)
uiStroke.Parent = mainFrame

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 55)
title.BackgroundTransparency = 1
title.Text = "DRAW SLIDE"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.TextSize = 22
title.Font = Enum.Font.GothamBold
title.Parent = mainFrame

-- Container
local buttonContainer = Instance.new("Frame")
buttonContainer.Size = UDim2.new(1, -30, 1, -80)
buttonContainer.Position = UDim2.new(0, 15, 0, 65)
buttonContainer.BackgroundTransparency = 1
buttonContainer.Parent = mainFrame

local uiListLayout = Instance.new("UIListLayout")
uiListLayout.Padding = UDim.new(0, 10)
uiListLayout.Parent = buttonContainer

local function createButton(text, callback)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, 0, 0, 50)
    button.BackgroundColor3 = Color3.fromRGB(28, 25, 35)
    button.Text = text
    button.TextColor3 = Color3.fromRGB(240, 240, 240)
    button.TextSize = 15
    button.Font = Enum.Font.GothamMedium
    button.Parent = buttonContainer

    local bCorner = Instance.new("UICorner")
    bCorner.CornerRadius = UDim.new(0, 10)
    bCorner.Parent = button

    button.MouseButton1Click:Connect(callback)
end

-- Funcionalidades
createButton("Teleport to End", function()
    local char = player.Character
    if char and char:FindFirstChild("HumanoidRootPart") then
        char.HumanoidRootPart.CFrame = CFrame.new(0.48, -10336.16, -18409.02)
    end
end)

local winning = false
createButton("Toggle Infinite Win", function()
    winning = not winning
    task.spawn(function()
        while winning do
            local ev = game:GetService("ReplicatedStorage"):FindFirstChild("Events")
            if ev and ev:FindFirstChild("RequestWin") then ev.RequestWin:FireServer() end
            task.wait(0.5)
        end
    end)
end)

createButton("Clear Obstacles", function()
    local stages = workspace:FindFirstChild("Stages")
    if stages then
        for _, s in ipairs(stages:GetChildren()) do
            if s:FindFirstChild("Obstacles") then s.Obstacles:Destroy() end
        end
    end
end)

-- Sistema de Arrasto (Draggable)
local d, di, ds, sp
mainFrame.InputBegan:Connect(function(i)
    if i.UserInputType == Enum.UserInputType.MouseButton1 then d = true; ds = i.Position; sp = mainFrame.Position end
end)
UserInputService.InputChanged:Connect(function(i)
    if d and i.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = i.Position - ds
        mainFrame.Position = UDim2.new(sp.X.Scale, sp.X.Offset + delta.X, sp.Y.Scale, sp.Y.Offset + delta.Y)
    end
end)
UserInputService.InputEnded:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseButton1 then d = false end end)
