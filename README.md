-- Interface básica
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "ESP_GUI"
ScreenGui.Parent = game.CoreGui

-- Função para criar ESP
local function CreateESP(player)
    if player == game.Players.LocalPlayer then return end
    local Box = Drawing.new("Text")
    Box.Visible = false
    Box.Center = true
    Box.Outline = true
    Box.Font = 2
    Box.Size = 14
    Box.Color = Color3.fromRGB(255, 0, 0)

    local RunService = game:GetService("RunService")
    local Connection

    Connection = RunService.RenderStepped:Connect(function()
        if not player.Character or not player.Character:FindFirstChild("HumanoidRootPart") or not player:FindFirstChild("Team") then
            Box.Visible = false
            return
        end

        -- Team Check
        if player.Team == game.Players.LocalPlayer.Team then
            Box.Visible = false
            return
        end

        local pos, onScreen = workspace.CurrentCamera:WorldToViewportPoint(player.Character.HumanoidRootPart.Position)
        if onScreen then
            Box.Position = Vector2.new(pos.X, pos.Y - 20)
            Box.Text = player.Name
            Box.Visible = true
        else
            Box.Visible = false
        end
    end)

    player.AncestryChanged:Connect(function(_, parent)
        if not parent then
            Box:Remove()
            if Connection then Connection:Disconnect() end
        end
    end)
end

-- Ativar ESP para todos os jogadores atuais
for _, player in pairs(game.Players:GetPlayers()) do
    CreateESP(player)
end

-- Atualizar para novos jogadores
game.Players.PlayerAdded:Connect(function(player)
    CreateESP(player)
end)# Script-do-Andrey157
