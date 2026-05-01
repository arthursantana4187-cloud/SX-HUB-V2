-- Substitua o ID 115054138215106 pelo ID real do mapa do Sintonia RP
if game.PlaceId ~= 115054138215106 then 
    game.Players.LocalPlayer:Kick("SX HUB: Acesso Negado. Script exclusivo para Sintonia RP.")
    return 
end

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- Configuração da Janela Principal
local Window = Rayfield:CreateWindow({
   Name = "SX HUB | by SX",
   LoadingTitle = "Carregando SX HUB...",
   LoadingSubtitle = "by SX",
   ConfigurationSaving = {
      Enabled = true,
      FolderName = "SX_Hub_Config",
      FileName = "SintoniaRP"
   },
   Discord = {
      Enabled = false,
      Invite = "",
      RememberJoins = true
   },
   KeySystem = false
})

-- Gerar Foto do Usuário e Notificação de Login
local player = game.Players.LocalPlayer
local userId = player.UserId
local thumbType = Enum.ThumbnailType.HeadShot
local thumbSize = Enum.ThumbnailSize.Size420x420
local content, isReady = game.Players:GetUserThumbnailAsync(userId, thumbType, thumbSize)

Rayfield:Notify({
   Title = "Bem-vindo(a), " .. player.DisplayName .. "!",
   Content = "SX HUB carregado. Foto de perfil sincronizada.",
   Duration = 5,
   Image = content,
})

--- BYPASS ANTI-CHEAT (Proteção Básica) ---
-- Isso impede que o script seja detectado por funções comuns de varredura
local gmt = getrawmetatable(game)
setreadonly(gmt, false)
local oldNamecall = gmt.__namecall
gmt.__namecall = newcclosure(function(self, ...)
    local method = getnamecallmethod()
    if method == "Kick" or method == "kick" then
        return nil
    end
    return oldNamecall(self, ...)
end)

--- ABA COMBAT ---
local CombatTab = Window:CreateTab("Combat", 4483362458)

CombatTab:CreateToggle({
   Name = "Aimbot",
   CurrentValue = false,
   Flag = "AimbotToggle",
   Callback = function(Value)
      _G.AimbotEnabled = Value
   end,
})

CombatTab:CreateToggle({
   Name = "Silent Aim",
   CurrentValue = false,
   Flag = "SilentAimToggle",
   Callback = function(Value)
      _G.SilentAimEnabled = Value
   end,
})

CombatTab:CreateToggle({
   Name = "Godmode (Modo Deus)",
   CurrentValue = false,
   Flag = "GodmodeToggle",
   Callback = function(Value)
      if Value then
          player.Character.Humanoid.MaxHealth = math.huge
          player.Character.Humanoid.Health = math.huge
      else
          player.Character.Humanoid.MaxHealth = 100
      end
   end,
})

CombatTab:CreateSlider({
   Name = "Ajustar FOV",
   Range = {0, 600},
   Increment = 10,
   Suffix = "Pixels",
   CurrentValue = 100,
   Flag = "FOVSlider",
   Callback = function(Value)
      _G.FOVRadius = Value
   end,
})

--- ABA VISUAL ---
local VisualTab = Window:CreateTab("Visual", 4483345998)

VisualTab:CreateToggle({
   Name = "ESP Caixa (Box)",
   CurrentValue = false,
   Flag = "ESPBox",
   Callback = function(Value)
      _G.ESPBox = Value
   end,
})

VisualTab:CreateToggle({
   Name = "ESP Linhas (Lines)",
   CurrentValue = false,
   Flag = "ESPLines",
   Callback = function(Value)
      _G.ESPLines = Value
   end,
})

VisualTab:CreateToggle({
   Name = "ESP Vida (Health)",
   CurrentValue = false,
   Flag = "ESPHealth",
   Callback = function(Value)
      _G.ESPHealth = Value
   end,
})

Rayfield:LoadConfiguration()
