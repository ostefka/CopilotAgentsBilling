---
layout: default
title: Fakturace Copilot Chat agentů
---

# Jak nastavit fakturaci pro Copilot Chat agenty (Microsoft 365 Admin Center)

> **Cílová skupina:** IT administrátoři / správci tenantů
> **Zaměření:** Konfigurace krok za krokem (bez informací o cenách)
> **Portál:** Microsoft 365 admin center (admin.microsoft.com)

---

## Úvod

Uživatelé Microsoft 365, kteří nemají placenou licenci Copilot, mohou přistupovat ke Copilot agentům a používat je prostřednictvím modelu průběžné fakturace (pay-as-you-go). Jako administrátor vše nakonfigurujete přímo v **Microsoft 365 admin center** — není nutné přistupovat do Power Platform admin center.

Tento článek Vás provede kompletním procesem nastavení, krok za krokem.

---

## Předpoklady

- Role Global Admin, Billing Admin nebo ekvivalentní role ve Vašem Microsoft 365 tenantovi
- Aktivní **Azure subscription** ve Vašem tenantovi (s oprávněním Owner nebo Contributor)
- **Resource group** v dané Azure subscription (pokud ji nemáte, vytvořte ji v Azure portálu)

---

## Krok 1: Přejděte na Copilot Billing & Usage

V Microsoft 365 admin center přejděte na **Copilot > Billing & usage**.

![Screenshot 1 - Billing & usage](screenshots/01.png)

Screenshot zobrazuje **Microsoft 365 admin center** s rozbalenou levou navigací v sekci **Copilot**. Je vybraná sekce **Billing & usage**, která zobrazuje hlavní stránku správy fakturace.

Klíčové prvky na obrazovce:
- **Levá navigace:** V sekci **Copilot** vidíte: Overview, Connectors, Search a **Billing & usage** (zvýrazněno/vybráno). Pod tím je sekce **Agents** s položkami Overview, All agents, Tools a Settings.
- **Hlavní oblast:** Zobrazuje se stránka **„Billing & usage"** s popisem: *„Set up pay-as-you-go billing by connecting apps and services to billing policies. View usage insights and costs for pay-as-you-go billing."*
- **Dvě záložky:** **Billing policies** (aktivní) a **Pay-as-you-go services**.
- **Prázdný stav:** Stránka zobrazuje *„You don't have any billing policies"* s výzvou k nastavení billing policies pro Microsoft 365 pay-as-you-go služby.
- **Akční tlačítka:** Máte dva způsoby, jak začít — odkaz **„+ Add a billing policy"** v horní části tabulky, nebo tlačítko **„Add a billing policy"** (červené) ve spodní části.

Klikněte na **„Add a billing policy"** a zahajte nastavení.

---

## Krok 2: Vyplňte fakturační údaje

Po kliknutí na **„Add a billing policy"** se zobrazí průvodce **Add billing policy**. Průvodce má čtyři kroky zobrazené v levém panelu:

1. **Billing details** (aktuální krok)
2. Choose users
3. Budget
4. Review and finish

![Screenshot 2 - Billing details](screenshots/02.png)

Na stránce **Billing details** vyplníte následující informace:

- **Name:** Zadejte popisný název billing policy (např. *„CopilotChatAgents"*). Tento název se rovněž zobrazí jako Power Platform account resource ve Vaší Azure subscription.

Sekce **Subscription details** — zde propojíte policy s Azure infrastrukturou:

- **Subscription:** Vyberte Azure subscription, vůči které chcete fakturovat. Musíte mít roli **Owner** nebo **Contributor** na Azure subscription i resource group. Pokud ještě nemáte subscription, klikněte na *„Create a new subscription"*.
- **Resource group:** Vyberte existující resource group v rámci dané subscription (např. *„CopilotChatAgents"*). Pokud žádnou nemáte, klikněte na *„Create a new resource group"* — budete přesměrováni do Azure portálu.
- **Region:** Vyberte region pro billing policy (např. *„North Europe"*).

**Terms of service** — ve spodní části zkontrolujte odkazy na podmínky služby:
- Pay-as-you-go billing terms of service
- Privacy statement

Zaškrtněte políčko **„I accept the pay-as-you-go billing terms of service"** a klikněte na **Next**.

---

## Krok 3: Výběr uživatelů

Druhým krokem průvodce je **Choose users**. Zde určíte, kteří uživatelé ve Vaší organizaci budou mít přístup k pay-as-you-go službám v rámci této billing policy.

> **Poznámka:** Tato volba se vztahuje pouze na **Microsoft 365 Copilot pay-as-you-go služby**.

![Screenshot 3 - Choose users](screenshots/03.png)

Máte dvě možnosti:

- **All users** (výchozí volba) — billing policy se vztahuje na všechny uživatele ve Vaší organizaci. Kdokoli může používat Copilot agenty a spotřeba se účtuje na propojenou Azure subscription.
- **Specific group** — omezení přístupu na jednu bezpečnostní skupinu. Pouze členové této skupiny budou moci využívat pay-as-you-go služby. To je užitečné, pokud chcete službu nejprve pilotně otestovat s menší skupinou nebo kontrolovat náklady omezením počtu uživatelů.

Vyberte požadovanou možnost a klikněte na **Next** pro přechod ke kroku Budget.

---

## Krok 4: Nastavení rozpočtu

Třetím krokem průvodce je **Budget**. Tento krok je volitelný, ale velmi doporučený — umožňuje nastavit limit výdajů a konfigurovat upozornění, abyste nebyli překvapeni neočekávanými náklady.

![Screenshot 4 - Budget](screenshots/04.png)

Zaškrtněte **„Set a budget for this policy"** pro aktivaci rozpočtu. Poté nakonfigurujte:

- **Budget ($ USD):** Zadejte maximální částku rozpočtu (např. *$100*). Jedná se o notifikační práh — **nezastaví** výdaje, ale spustí upozornění.

- **Reset the budget:** Zvolte, jak často se rozpočet resetuje:
  - **On the first day of the month** (nejběžnější, výchozí volba)
  - On the first day of the quarter
  - On the first day of the year

- **Send email alerts:**
  - **Recipients:** Přidejte uživatele nebo skupiny, kteří mají dostávat upozornění na náklady. V tomto příkladu byla přidána skupina *„AgentsAlerts"*. Můžete vyhledat další skupiny.
  - **Send alerts when usage reaches this percentage of budget:** Nakonfigurujte jeden nebo více prahů. Výchozí upozornění na **100 %** ($100.00) je vždy přítomno. Můžete přidat další prahy — například **80 %** ($80.00) pro včasné varování. Klikněte na **„+ Add more"** pro přidání dalších úrovní.

> **Tip:** Nastavení prahu na 80 % dá Vašemu týmu prostor reagovat dříve, než dojde k vyčerpání celého rozpočtu.

Klikněte na **Next** pro přechod na závěrečný kontrolní krok.

---

## Krok 5: Kontrola a vytvoření policy

Posledním krokem je **Review and finish**. Na této stránce se zobrazí souhrn celé konfigurace, abyste mohli vše ověřit před vytvořením policy.

![Screenshot 5 - Review and create policy](screenshots/05.png)

Souhrn zobrazuje:

- **Policy name:** CopilotChatAgents
- **Azure subscription:** Propojená Azure subscription (částečně zakrytá z důvodu ochrany soukromí)
- **Resource group:** CopilotChatAgents
- **Region:** North Europe
- **Users:** All users
- **Budget details:** $100.00

Každá sekce má odkaz **„Edit"** (např. *Edit policy name*, *Edit Azure subscription*, *Edit resource group*, *Edit region*, *Edit users*, *Edit budget*), který Vám umožní vrátit se a upravit konkrétní nastavení bez nutnosti restartovat celého průvodce.

Jakmile jste ověřili správnost všech údajů, klikněte na tlačítko **„Create policy"**.

---

## Krok 6: Přidání další billing policy — SharePoint Agents

Můžete vytvořit více billing policies pro různé účely. V tomto příkladu přidáváme druhou policy speciálně pro **SharePoint Agents**.

Průvodce je stejný jako předtím — zobrazí se stránka **Billing details** se stejnými čtyřmi kroky.

![Screenshot 6 - SharePoint Agents billing details](screenshots/06.png)

Tentokrát jsou pole nakonfigurována pro SharePoint Agents:

- **Name:** *SharePointAgents*
- **Subscription:** Stejná Azure subscription jako předtím (nebo jiná, pokud preferujete oddělení nákladů)
- **Resource group:** *SharePointAgents* — dedikovaná resource group pro oddělení nákladů na SharePoint agenty od Copilot Chat
- **Region:** North Europe
- **Terms of service:** Akceptováno

> **Tip:** Používání oddělených billing policies s dedikovanými resource groups je osvědčený postup pro řízení nákladů. Umožňuje přehledně sledovat a oddělovat náklady mezi různými typy agentů (např. Copilot Chat vs. SharePoint Agents) v Azure cost analysis.

Klikněte na **Next** a pokračujte průvodcem (Choose users, Budget, Review) stejným způsobem jako u první policy.

---

## Krok 7: Přehled billing policies

Po vytvoření obou policies se vrátíte na stránku **Billing & usage**. Místo prázdného stavu nyní vidíte seznam Vašich billing policies.

![Screenshot 7 - Billing policies list](screenshots/07.png)

Tabulka zobrazuje všechny vytvořené billing policies s následujícími sloupci:

| Name | Users | Services | Budget used |
|------|-------|----------|-------------|
| CopilotChatAgents | All users | Connect a service | 0% |
| SharePointAgents | All users | Connect a service | 0% |

Klíčová pozorování:

- Sloupec **Services** zobrazuje **„Connect a service"** u obou policies — to znamená, že policies jsou vytvořeny a propojeny s Azure, ale zatím nebyla připojena žádná pay-as-you-go služba. Je třeba kliknout na **„Connect a service"** a aktivovat fakturaci pro konkrétní službu (např. Microsoft 365 Copilot Chat, SharePoint agents).
- **Budget used** zobrazuje **0 %** u obou — dosud nebyla zaznamenána žádná spotřeba.
- Stále můžete přidat další billing policies pomocí odkazu **„+ Add a billing policy"** v horní části.

Dalším krokem je propojení služby s každou billing policy.

---

## Krok 8: Záložka Pay-as-you-go services

Přepněte na záložku **Pay-as-you-go services**, kde uvidíte dostupné služby, které lze propojit s billing policies.

![Screenshot 8 - Pay-as-you-go services](screenshots/08.png)

Toto zobrazení ukazuje:

| Service | Billing policy | Connected to |
|---------|---------------|--------------|
| Microsoft 365 Copilot Chat | None | None |
| SharePoint agents | None | None |

K dispozici jsou dvě služby:
- **Microsoft 365 Copilot Chat** — pro využití Copilot Chat agentů uživateli M365
- **SharePoint agents** — pro využití SharePoint agentů

U obou se aktuálně zobrazuje **„None"** ve sloupcích Billing policy a Connected to — což znamená, že zatím nejsou propojeny s žádnou billing policy.

V horní části stránky máte také rychlé odkazy:
- **View cost management** — otevře Azure Cost Management pro analýzu výdajů
- **Manage billing policies** — přesměruje zpět na záložku Billing policies

Klikněte na řádek služby a propojte ji s jednou z dříve vytvořených billing policies.

---

## Krok 9: Propojení billing policy s Microsoft 365 Copilot Chat

Po kliknutí na **Microsoft 365 Copilot Chat** v seznamu služeb se otevře boční panel: **„Manage billing for Microsoft 365 Copilot Chat"**.

![Screenshot 9 - Manage billing for M365 Copilot Chat](screenshots/09.png)

Panel má dvě sekce:

### Copilot Credits
V horní části můžete využít **Copilot Credits**, které byly přiděleny prostředí Microsoft 365 Copilot Chat Power Platform. Můžete vybrat existující Copilot Credit policies nebo vytvořit novou přes **„+ Add new Copilot Credit policy"**. V tomto příkladu se zobrazuje *„No Copilot Credit policies yet"* — což znamená, že se spoléháme výhradně na pay-as-you-go fakturaci.

> **Důležité:** Pokud je uživatel součástí jak Copilot Credit policy, tak pay-as-you-go billing policy, kredity se vyčerpají jako první a pay-as-you-go fakturace se použije pouze v případě, že nejsou k dispozici žádné kredity.

### Billing policies
Zde propojíte službu s Vaší billing policy. Klíčové prvky:

- **Informační zpráva:** *„You're connecting a billing policy to all current, and future agents in Microsoft 365 Copilot Chat category"* — propojení se vztahuje na všechny agenty v této kategorii.
- **Zaškrtávací políčko:** *„Use any available capacity pack credits for users in connected billing policies. If no credits are available, the billing policy's Azure subscription is charged."* — tím zajistíte, že se nejprve využijí předplacené kredity.

Tabulka zobrazuje Vaše billing policies:

| Billing policy name | Connection status | Included in policy | Azure subscription | Resource group |
|---------------------|------------------|-------------------|-------------------|----------------|
| CopilotChatAgents | **Connected** (přepínač ZAP) | All users | ME-M365CPI270401... | CopilotChatAgents |
| SharePointAgents | Disconnected (přepínač VYP) | All users | ME-M365CPI270401... | SharePointAgents |

Přepněte **Connection status** na **Connected** u policy **CopilotChatAgents** a klikněte na **Save**.

---

## Krok 10: Propojení billing policy se SharePoint Agents

Nyní proveďte totéž pro službu **SharePoint agents**. Klikněte na **SharePoint agents** v seznamu služeb a otevře se boční panel: **„Manage billing for SharePoint agents"**.

![Screenshot 10 - Manage billing for SharePoint agents](screenshots/10.png)

Všimněte si, že na pozadí vlevo služba **Microsoft 365 Copilot Chat** nyní zobrazuje **CopilotChatAgents** jako svou billing policy a **All users** ve sloupci Connected to — potvrzení, že předchozí krok byl úspěšně uložen.

Boční panel má identickou strukturu jako předchozí, ale je zaměřen na **SharePoint agents**:

- **Informační zpráva:** *„You're connecting a billing policy to all current, and future agents in **SharePoint agents** category."*
- **Zaškrtávací políčko:** Stejná možnost využití předplacených kreditů před Azure fakturací.

Tentokrát přepněte policy **SharePointAgents** na **Connected** a ponechte CopilotChatAgents jako **Disconnected**:

| Billing policy name | Connection status | Included in policy | Azure subscription | Resource group |
|---------------------|------------------|-------------------|-------------------|----------------|
| CopilotChatAgents | Disconnected | All users | ME-M365CPI270401... | CopilotChatAgents |
| SharePointAgents | **Connected** (přepínač ZAP) | All users | ME-M365CPI270401... | SharePointAgents |

Klikněte na **Save**.

> **Klíčový poznatek:** Použitím oddělených billing policies pro každou službu získáte čisté oddělení nákladů v Azure — spotřeba Copilot Chat se směřuje do resource group *CopilotChatAgents* a spotřeba SharePoint agentů do resource group *SharePointAgents*. To výrazně zjednodušuje sledování nákladů a interní přeúčtování.

---

## Krok 11: Výsledná konfigurace — všechny služby propojeny

Po uložení obou propojení záložka **Pay-as-you-go services** zobrazuje dokončenou konfiguraci.

![Screenshot 11 - Final configuration](screenshots/11.png)

Vše je nyní plně propojeno:

| Service | Billing policy | Connected to |
|---------|---------------|--------------|
| Microsoft 365 Copilot Chat | CopilotChatAgents | All users |
| SharePoint agents | SharePointAgents | All users |

Obě služby jsou propojeny s příslušnými billing policies a jsou dostupné pro **All users** v organizaci.

**Hotovo — nastavení je kompletní!** Od tohoto okamžiku:
- Veškerá spotřeba Copilot Chat agentů uživateli M365 bude účtována na Azure subscription pod resource group *CopilotChatAgents*.
- Veškerá spotřeba SharePoint agentů bude účtována pod resource group *SharePointAgents*.
- Výdaje můžete sledovat přes **„View cost management"** (odkaz na Azure Cost Management) nebo na záložce **Billing policies** podle procentuálního využití rozpočtu.

---

## Shrnutí

Pro aktivaci pay-as-you-go fakturace pro Copilot Chat a SharePoint agenty z **Microsoft 365 admin center**:

1. Přejděte na **Copilot > Billing & usage**
2. Vytvořte **billing policies** — jednu pro každou službu/nákladové středisko (s propojením na Azure subscription + resource group)
3. Nakonfigurujte **rozsah uživatelů** (všichni uživatelé nebo konkrétní skupina)
4. Nastavte **rozpočty** a **prahy upozornění** pro předcházení nečekaným nákladům
5. Přepněte na záložku **Pay-as-you-go services** a **propojte** každou službu s její billing policy

> **Osvědčený postup:** Používejte oddělené billing policies a resource groups pro každou službu — dosáhnete tak čistého oddělení nákladů a snadnější analýzy v Azure.

---

## Poznámky

- Tento postup je určen specificky pro **Copilot Chat agenty** a **SharePoint agenty** (uživatelé M365 bez placené licence Copilot)
- Vše se provádí z **M365 admin center** (admin.microsoft.com), NIKOLI z Power Platform admin center
- Copilot Studio agenti mají odlišný postup nastavení fakturace (popsaný v samostatném článku)
