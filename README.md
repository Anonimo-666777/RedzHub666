# redzlib V6 — Documentation

> UI Library feita por 
Menkato e David
**redz9999**.  
> Versão: `6`

---

## 📦 Carregando a Library

```lua
local redzlib = loadstring(game:HttpGet("https://pastefy.app/2HvS5nD4/raw"))()
```

---

## 🪟 MakeWindow

Cria a janela principal do hub.

```lua
local Window = redzlib:MakeWindow({
    "Meu Hub",           -- [1] Título principal
    "by : redz9999",     -- [2] Subtítulo
    "MeuScript.json"     -- [3] Arquivo de save (opcional)
})
```

| Parâmetro | Alias | Tipo | Descrição |
|-----------|-------|------|-----------|
| `[1]` | `Name` / `Title` | string | Título da janela |
| `[2]` | `SubTitle` | string | Subtítulo abaixo do título |
| `[3]` | `SaveFolder` | string | Nome do arquivo `.json` para salvar flags |

### Métodos de Window

```lua
Window:Set("Novo Título", "Novo Sub")   -- Muda título e subtítulo
Window:Minimize()                        -- Toggle visibilidade da janela
Window:Dialog({ ... })                   -- Abre um dialog (veja abaixo)
Window:AddMinimizeButton({ ... })        -- Adiciona botão flutuante de minimizar
```

---

## 📑 AddTab

Adiciona uma aba na sidebar.

```lua
local Tab = Window:AddTab({
    "Nome da Aba",       -- [1] Nome
    "star"               -- [2] Ícone (nome Lucide ou rbxassetid://)
})
```

| Parâmetro | Alias | Tipo | Descrição |
|-----------|-------|------|-----------|
| `[1]` | `Name` / `Title` | string | Nome da aba |
| `[2]` | `Image` / `Icon` | string | Ícone da aba |

### Métodos de Tab

```lua
Tab:Enable()           -- Ativa/seleciona a aba
Tab:Disable()          -- Desativa a aba
Tab:Visible(bool)      -- Mostra/esconde a aba na sidebar
Tab:Destroy()          -- Remove a aba completamente
```

---

## 📌 AddSection

Separador de seção dentro de uma aba.

```lua
local Section = Tab:AddSection("Nome da Seção")
-- ou
local Section = Tab:AddSection({ "Nome da Seção" })
```

### Métodos de Section

```lua
Section:Set("Novo Nome")     -- Muda o texto da seção
Section:Visible(bool)        -- Mostra/esconde
Section:Destroy()            -- Remove
```

---

## 📝 AddParagraph

Exibe um bloco de texto com título e descrição.

```lua
local Para = Tab:AddParagraph({
    "Título do Parágrafo",   -- [1] Title
    "Descrição aqui..."      -- [2] Text
})
```

### Métodos de Paragraph

```lua
Para:SetTitle("Novo Título")
Para:SetDesc("Nova Descrição")
Para:Set("Título", "Descrição")   -- Define os dois de uma vez
Para:Visible(bool)
Para:Destroy()
```

---

## 🔘 AddButton

Botão clicável.

```lua
local Btn = Tab:AddButton({
    "Clique Aqui",           -- [1] Nome
    function()               -- [2] Callback
        print("clicou!")
    end,
    Desc = "Descrição opcional"
})
```

| Parâmetro | Alias | Tipo | Descrição |
|-----------|-------|------|-----------|
| `[1]` | `Name` / `Title` | string | Nome do botão |
| `[2]` | `Callback` | function | Função chamada ao clicar |
| `Desc` | `Description` | string | Descrição abaixo do nome |

### Métodos de Button

```lua
Btn:Set("Novo Nome")                     -- Muda o nome
Btn:Set("Novo Nome", "Nova Desc")        -- Muda nome e descrição
Btn:Set(function() ... end)              -- Troca o callback
Btn:Callback(function() ... end)         -- Adiciona callback extra
Btn:Visible(bool)
Btn:Destroy()
```

---

## ✅ AddToggle

Toggle on/off com suporte a flags e save.

```lua
local Toggle = Tab:AddToggle({
    "Ativar Speed",      -- [1] Nome
    false,               -- [2] Default (true/false)
    function(val)        -- [3] Callback
        print("Toggle:", val)
    end,
    Flag = "SpeedFlag",  -- Flag para salvar o estado
    Desc = "Liga o speed"
})
```

| Parâmetro | Alias | Tipo | Descrição |
|-----------|-------|------|-----------|
| `[1]` | `Name` / `Title` | string | Nome do toggle |
| `[2]` | `Default` | boolean | Estado inicial |
| `[3]` | `Callback` | function | Função chamada com `true`/`false` |
| `[4]` | `Flag` | string | Nome da flag de save |
| `Desc` | `Description` | string | Descrição |

### Métodos de Toggle

```lua
Toggle:Set(true)                          -- Define estado (boolean)
Toggle:Set("Novo Nome")                   -- Muda o nome
Toggle:Set("Nome", "Desc")               -- Muda nome e descrição
Toggle:Set(function(v) ... end)          -- Troca callback
Toggle:Callback(function() ... end)      -- Adiciona callback extra
Toggle:Visible(bool)
Toggle:Destroy()
```

---

## 🔽 AddDropdown

Dropdown com suporte a seleção múltipla.

```lua
local Drop = Tab:AddDropdown({
    "Escolha um item",       -- [1] Nome
    {"Item 1", "Item 2"},    -- [2] Opções
    "Item 1",                -- [3] Default
    function(val)            -- [4] Callback
        print("Selecionado:", val)
    end,
    Flag = "DropFlag",
    MultiSelect = false,
    Desc = "Selecione algo"
})
```

| Parâmetro | Alias | Tipo | Descrição |
|-----------|-------|------|-----------|
| `[1]` | `Name` / `Title` | string | Nome do dropdown |
| `[2]` | `Options` | table | Lista de opções |
| `[3]` | `Default` | string / table | Opção padrão |
| `[4]` | `Callback` | function | Função chamada com o valor |
| `[5]` | `Flag` | string | Flag de save |
| `MultiSelect` | — | boolean | Permite múltipla seleção |

### Métodos de Dropdown

```lua
Drop:Set({"Item 2"})                     -- Define seleção
Drop:Set("Novo Nome")                    -- Muda o nome
Drop:Refresh({"A", "B", "C"})           -- Atualiza as opções
Drop:Callback(function(v) ... end)       -- Adiciona callback
Drop:Visible(bool)
Drop:Destroy()
```

---

## 🎚️ AddSlider

Slider numérico com precisão decimal.

```lua
local Slider = Tab:AddSlider({
    "Velocidade",        -- [1] Nome
    {0, 100},            -- [2] {Min, Max}
    50,                  -- [3] Default
    function(val)        -- [4] Callback
        print("Valor:", val)
    end,
    Flag = "SpeedSlider",
    Suffix = "x",
    Increase = 1,        -- Precisão (ex: 0.1 para decimais)
    Desc = "Ajuste a velocidade"
})
```

| Parâmetro | Alias | Tipo | Descrição |
|-----------|-------|------|-----------|
| `[1]` | `Name` / `Title` | string | Nome do slider |
| `[2]` | `Range` | table `{min, max}` | Intervalo de valores |
| `[3]` | `Default` | number | Valor inicial |
| `[4]` | `Callback` | function | Função chamada com o valor |
| `[5]` | `Flag` | string | Flag de save |
| `Suffix` | — | string | Texto após o valor (ex: `"x"`) |
| `Increase` | — | number | Passo/precisão do slider |

### Métodos de Slider

```lua
Slider:Set(75)                           -- Define valor numérico
Slider:Set("Novo Nome")                  -- Muda o nome
Slider:Set("Nome", "Desc")              -- Muda nome e descrição
Slider:Callback(function(v) ... end)    -- Adiciona callback
Slider:Visible(bool)
Slider:Destroy()
```

---

## ⌨️ AddTextBox

Campo de texto para entrada de dados.

```lua
local Box = Tab:AddTextBox({
    "Digite algo",           -- [1] Nome
    "",                      -- [2] Default
    false,                   -- [3] ClearTextOnFocus
    function(val)            -- [4] Callback
        print("Texto:", val)
    end,
    PlaceholderText = "Digite aqui...",
    Desc = "Descrição"
})
```

| Parâmetro | Alias | Tipo | Descrição |
|-----------|-------|------|-----------|
| `[1]` | `Name` / `Title` | string | Nome do textbox |
| `[2]` | `Default` | string | Texto padrão |
| `[3]` | `ClearText` | boolean | Limpar ao focar |
| `[4]` | `Callback` | function | Chamado ao confirmar o texto |
| `[5]` | `PlaceholderText` | string | Placeholder |

### Métodos de TextBox

```lua
Box.OnChanging = function(text)
    return text:upper()      -- Transforma o texto antes do callback
end
Box:Visible(bool)
Box:Destroy()
```

---

## 💬 AddDiscordInvite

Exibe um card de convite do Discord.

```lua
Tab:AddDiscordInvite({
    "Nome do Servidor",      -- [1] Título
    "rbxassetid://...",      -- [2] Logo
    "discord.gg/invite"      -- [3] Link do convite
})
```

Ao clicar no botão **Join**, o link é copiado para o clipboard automaticamente.

---

## 🪟 Dialog

Abre um popup de confirmação.

```lua
local Dialog = Window:Dialog({
    "Título",                -- [1] Título
    "Mensagem aqui",         -- [2] Texto
    {                        -- [3] Botões
        {"Confirmar", function()
            print("confirmado!")
        end},
        {"Cancelar"}
    }
})
```

### Métodos de Dialog

```lua
Dialog:Button({"Nome", function() ... end})   -- Adiciona botão extra
Dialog:Close()                                 -- Fecha o dialog
```

---

## 🎨 Temas disponíveis

| Nome | Descrição |
|------|-----------|
| `DarkRed` | Vermelho escuro intenso |
| `Volcano` | Laranja vulcânico |
| `JJMods` | Azul estilo executor |
| `Delta` | Dark com detalhes azuis |
| `Skibx` | Roxo vibrante |
| `Arceus` | Dourado |
| `Wave` | Ciano/Teal |
| `Solara` | Rosa/Magenta |
| `Dark` | Cinza escuro clássico |
| `Purple` | Roxo suave |

### Mudando o tema

```lua
redzlib:SetTheme("Delta")
```

---

## 🔧 Funções globais da library

```lua
-- Retorna o rbxassetid de um ícone pelo nome Lucide
redzlib:GetIcon("star")         -- "rbxassetid://10734966248"

-- Muda o tema da UI em tempo real
redzlib:SetTheme("Wave")

-- Ajusta a escala da UI (valor do viewport Y)
redzlib:SetScale(600)
```

---

## 🚩 Sistema de Flags

Flags salvam o estado dos elementos entre sessões no arquivo `.json` configurado no `MakeWindow`.

```lua
-- Lendo uma flag manualmente
local val = redzlib.Flags["MinhaFlag"]

-- Ouvindo mudanças de flags
redzlib.Connection.FlagsChanged:Connect(function(flag, value)
    print(flag, "mudou para:", value)
end)
```

---

## 🔗 Sistema de Conexões

A library expõe eventos internos via `redzlib.Connection`:

| Evento | Dispara quando... |
|--------|------------------|
| `FlagsChanged` | Uma flag muda de valor |
| `ThemeChanged` | O tema é alterado |
| `FileSaved` | O arquivo de save é escrito |

```lua
redzlib.Connection.ThemeChanged:Connect(function(theme)
    print("Novo tema:", theme)
end)
```

---

## 📋 Exemplo completo

```lua
local redzlib = loadstring(game:HttpGet("SEU_LINK"))()

local Window = redzlib:MakeWindow({
    "Meu Hub",
    "by : eu mesmo",
    "MeuHub.json"
})

local Tab = Window:AddTab({"Principal", "star"})

Tab:AddSection("Combate")

Tab:AddToggle({
    "God Mode",
    false,
    function(val)
        -- seu código aqui
    end,
    Flag = "GodMode"
})

Tab:AddSlider({
    "WalkSpeed",
    {16, 500},
    16,
    function(val)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = val
    end,
    Suffix = " stud/s"
})

Tab:AddButton({
    "Teleportar",
    function()
        -- teleporte aqui
    end
})

redzlib:SetTheme("DarkRed")
```
