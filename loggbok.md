# Loggbok – Frontendprojekt KJPJ

Individuell loggbok över arbetet med webbshoppen. Loggboken uppdateras efter varje sprint och beskriver arbetsuppgifter, tekniska lösningar och problem som uppstått under utvecklingen.

## Sprint 1

### FE-13 – Gemensam Header och Footer

Arbetet började med att skapa två separata komponenter, `Header.tsx` och `Footer.tsx`. Syftet var att få en gemensam layout som kunde återanvändas på flera sidor istället för att behöva skapa samma innehåll på varje sida.

Tailwind CSS användes för att styla komponenterna och anpassa deras utseende. Därefter integrerades Header och Footer i `App.tsx` så att de blev en del av den gemensamma layouten.

Layouten anpassades så att Header visas överst och Footer längst ned, även på sidor med lite innehåll. Placeringen testades på olika sidor för att kontrollera att layouten fungerade korrekt.

Utöver frontend-arbetet startades och testades även mikrotjänsterna lokalt på datorn för att kontrollera att de fungerade tillsammans med frontend.

## Sprint 2

### FE-12 – Skydda privata sidor

Arbetet handlade om att kontrollera användarens inloggningsstatus och förhindra att utloggade användare kunde komma åt privata sidor som WelcomePage och ProductsPage.

Olika situationer testades genom att öppna sidorna både via navigationen och genom att skriva in URL-adressen direkt i webbläsaren. Om användaren inte var inloggad skulle den skickas tillbaka till LoginPage.

Även navigationen kontrollerades så att länkar till privata sidor inte visades för utloggade användare. På så sätt styrs vilka delar av webbshoppen som är tillgängliga beroende på användarens inloggningsstatus.

### IMP-03-T04 – Anpassa skyddade sidor

Arbetet fortsatte med att anpassa skyddet för WelcomePage, Kundvagn och Ordrar. Dessa sidor ska endast vara tillgängliga för inloggade användare.

En viktig del var att testa direktnavigering via URL, eftersom det inte räcker att bara dölja länkarna i navigationen. Även om en användare skriver in adressen manuellt ska åtkomsten kontrolleras.

Utloggade användare ska därför skickas till LoginPage när de försöker öppna någon av de skyddade sidorna.

### IMP-03-T05 – Anpassa Header och navigation

Header anpassades efter användarens inloggningsstatus. För utloggade användare visas Logo, Produkter och Logga in. För inloggade användare visas även Kundvagn, Ordrar och Logga ut.

Navigationen ändras alltså beroende på om användaren är inloggad eller inte. Även placeringen av Header kontrollerades så att den visas på WelcomePage och övriga relevanta sidor.

## Sprint 3

### FE-14 – Undersöka betalningslösning för webbshoppen

Olika möjligheter för att lägga till betalning i webbshoppen undersöktes. Fokus låg på att jämföra betalningslösningar och ta reda på hur ett betalningsflöde skulle kunna fungera i applikationen.

Även vilka delar i frontend och backend som skulle behöva ändras undersöktes. Det handlade bland annat om hur kunden skulle kunna gå vidare från kundvagnen till betalningen och hur betalningsresultatet skulle hanteras.

Efter undersökningen föreslogs Stripe som betalningssystem. Förslaget presenterades för teamet och mottogs positivt. Beslutet blev att gå vidare med Stripe och arbeta med integrationen i kommande sprintar.

### FE-26 – Filtrera produkter efter kategori

Arbetet handlade om att lägga till möjligheten att filtrera produkter efter kategori på produktsidan. Syftet var att göra det enklare för kunden att hitta produkter utan att behöva gå igenom hela produktlistan.

Ett kategorival lades till så att användaren kan välja vilken kategori som ska visas. När en kategori väljs ska produktlistan uppdateras och endast visa produkter som tillhör den valda kategorin.

Det ska även gå att återgå till att visa alla produkter. En annan viktig del var att hantera situationer där det inte finns några produkter i den valda kategorin, så att användaren får tydlig information istället för en tom produktlista.

### FE-27 – Testa email-service lokalt

Email-service testades lokalt på datorn för att kontrollera att tjänsten kunde startas och att konfigurationen för e-post fungerade. Även anslutningen till RabbitMQ undersöktes.

Eftersom email-service kommunicerar med order-service via RabbitMQ var det viktigt att kontrollera att meddelanden kunde tas emot när en order skapas.

Målet var att testa flödet från att en order skapas till att email-service tar emot orderhändelsen och skickar ett orderbekräftelsemejl till kunden. På så sätt kunde e-postfunktionen testas lokalt utan att tjänsterna behövde driftsättas.

## Sprint 4

### FE-29 – Skapa en 404-sida

En 404-sida skapades för situationer där användaren försöker öppna en webbadress som inte finns i webbshoppen.

Syftet var att visa ett tydligt felmeddelande istället för att användaren hamnar på en tom sida eller inte får någon återkoppling. En separat felsida implementerades för okända URL-adresser.

Efter implementationen testades sidan genom att skriva in felaktiga URL-adresser direkt i webbläsaren. På så sätt kunde det kontrolleras att 404-sidan visades istället för en vanlig sida.

## Sprint 5

### FE-39 – Konfigurera MSW

För att kunna testa frontend utan att behöva starta backendens mikrotjänster installerades MSW (Mock Service Worker) i frontendprojektet med kommandot:

`npm install msw --save-dev`

Därefter skapades en handler-fil med mockade API-anrop för `/auth/me` och `/products` med hjälp av `http.get()`. Dessa handlers används för att simulera olika svar från backend när frontend gör API-anrop.

En setup-fil skapades också för testmiljön. Där konfigurerades MSW att starta före testerna med `server.listen()`, återställa handlers efter varje test med `server.resetHandlers()` och stängas av efter testningen med `server.close()`.

Under konfigurationen uppstod problem med att MSW även startade när frontendapplikationen kördes vanligt. Det krävdes därför felsökning för att skilja testmiljön från den vanliga utvecklingsmiljön, så att mockningen endast används vid testning.

### FE-40 – Testa produkthämtning

Efter konfigurationen av MSW fortsatte arbetet med att testa produkthämtningen med Vitest och React Testing Library.

Först skapades testdata för produkter som kunde användas under testningen. Därefter användes MSW för att mocka API-anropet till `/products`, så att frontend fick ett simulerat svar från backend.

Tester skrevs för att kontrollera att produkterna hämtades och visades korrekt på produktsidan när API-anropet lyckades. Med React Testing Library kunde det kontrolleras vad som faktiskt visades i komponenten efter att API-svaret hade kommit tillbaka.

Till sist kördes testerna lokalt med kommandot `npm test`. Även de tidigare testerna kontrollerades för att säkerställa att de fortfarande fungerade tillsammans med de nya testerna.
