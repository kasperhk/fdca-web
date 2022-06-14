---
Week: 07
tags:
  - gitlab
  - gitlab pages
  - gitlab runner
  - git
  - ssh
  - ssh user agent
  - bash
  - python
---

# Øvelser uge 7

Denne uges øvelser har fokus på teknik og samarbejde.

Dagens øvelser afsluttes med refleksions spørgsmål som du skal besvare i dine private noter.  
De kan vise sig at blive værdifulde som forberedelse til eksamen men også som en måde at fastholde din viden.

Husk at stille mig og hinanden spørgsmål, kun ved at spørge kan vi blive klogere i fællesskab.

## Øvelse 1 - Git/gitlab og ssh nøgler

### Information

I denne øvelse skal du opsætte git på din computer, lave ssh nøgler og forbinde til gitlab (og andre steder) med ssh.
Det hele er beskrevet i en guide som nok kræver at du læser lidt dokumentation forskellige steder. Guiden linker til de ressourcer du har brug for.  
Ud over det skal du også opsætte en user agent så du ikke behøver taste dit ssh key password hver gang du laver f.eks en git push.

SSH nøglepar består af en offentlig og en privat del. Den private nøgle har ikke nogen fil type og den offentlige har filtypen `.pub`

Du bør aldrig dele din private nøgle eller have den liggende andre steder end på din private maskine. Eneste undtagelse er en backup af din private nøgle, her vil jeg overlade det til dig at finde et egnet sted, en usb stick, en password manager eller andet.

Din offentlige SSH nøgle kan, som navnet antyder, deles med ofentligheden. For at tilgå andre maskiner eller services med SSH skal din offentlige nøgle være placeret på det sted du forsøger at tilgå.  
Det kan være lidt forskelligt hvor du placerer den når du bruger services som f.eks gitlab, på en remote maskine skal den ligge i `~/.ssh/authorized_keys` filen.

_Dette er en individuel øvelse, men i skal arbejde sammen i par når i laver øvelserne. Undervejs skal i hjælpe hinanden så i når i mål med det hele._

### Instruktioner

**Del 1**  
Følg denne guide for at opsætte git, gitlab og ssh nøgler [https://eal-itt.gitlab.io/gitlab_daily_workflow/index.html](https://eal-itt.gitlab.io/gitlab_daily_workflow/index.html) sørg for at klikke på alle links for at få så meget viden som muligt

**Del 2** ssh user agent

Som sagt i beskrivelsen til denne opgave kan du tilgå maskiner og services med ssh hvis du har tilføjet en offentlig ssh nøgle som matcher din private ssh nøgle.  
Det virker fint, men hvad hvis du har brug for at tilgå en 3. maskine, med ssh, fra den 2. maskine?  
Det vil ikke virke med mindre din private ssh nøgle er på den 2. maskine, men det er som sagt ikke en god ide at have din private nøgle andre steder end din private maskine.  
En løsning er at opsætte en _ssh user agent_ og tilføje din private nøgle til agenten. Agenten kan så _forwardes_ til en remote maskine og dermed gøre det muligt at tilgå yderlige maskiner fra den 2. maskine.  
Nå du afslutter din ssh session vil agenten trækkes tilbage og sørge for at din private ssh nøgle ikke er på andre maskiner mere.

I denne øvelse kommer du ikke længere end fordelen ved at have en user agent og slippe for at skrive dit password igen og igen. Med mindre du har en remote maskine kan du også prøve at forwarde din user agent til den maskine.

**Del 2.1 - opsæt ssh user agent og autostart**

1.  Åben git bash (eller anden terminal der kan køre git bash)
2.  Start din user agent `eval $(ssh-agent -s)`
3.  Tilføj din ssh nøgle til agenten `ssh-add ~/.ssh/id_ed25519`
4.  Opsæt user agent autostart ved at følge denne guide [https://docs.github.com/en/authentication/connecting-to-github-with-ssh/working-with-ssh-key-passphrases#auto-launching-ssh-agent-on-git-for-windows](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/working-with-ssh-key-passphrases#auto-launching-ssh-agent-on-git-for-windows)  
    Hvis du ikke har en `.bashrc` fil, så opret den i roden af din brugers home mappe `~/`

_Hvis du ikke opsætter autostart er du nød til manuelt at starte din agent hver gang din maskine genstartes med kommandoen `eval $(ssh-agent -s)`_

**Del 2.2 - forward ssh user agent**

1. Sørg for at din ssh user agent kører, hvis du har fulgt **del 2.1** så burde din user agent starte når du åbner din terminal, hvis ikke så gå tilbage til **del 2.1** og få det til at virke.
2. Åben en terminal og ssh til din remote maskine med syntaksen `ssh -A user@myhost.com`  
    myhost.com kan selvfølgelig også være en ip adresse. Brugeren er den bruger du har opsat på din remote maskine.  
    `-A` parametret fortæller ssh at agenten skal forwardes.  
   Du kan læse mere om ssh kommando parametre på [https://man.openbsd.org/ssh](https://man.openbsd.org/ssh) eller ved at skrive `man ssh` hvis du er på linux.
3. Fra din remote maskine test at din private nøgle er forwardet ved at verificere med gitlab.com, kør kommandoen `ssh -T git@gitlab.com`

## Øvelse 2 - gitlab pages til personlig portfolio

### Information

Gitlab giver mulighed for [Continuous Integration, Continuous Delivery and Continuous Deployment (CI/CD)](https://docs.gitlab.com/ee/ci/README.html).
Helt kort giver det mulighed for at bygge og teste software i docker containere.  
Gitlab kan også hoste statiske hjemmesider, alle gitlab projekter har mulighed for dette. Det kaldes [Gitlab pages](https://about.gitlab.com/stages-devops-lifecycle/pages/).

I denne øvelse kan du lære hvordan du bruger gitlab's CI/CD og gitlab pages til at bygge din personlige portfolio. Du skal bruge en statisk site generator som hedder mkdocs [https://www.mkdocs.org/](https://www.mkdocs.org/)  
Portfolio siden skal du bruge til at vise de ting du lærer i "Introduktion til it-sikkerhed".

mkdocs er en statisk site generator som oversætter markdown filer til HTML. Du behøver altså ikke at vide noget om at skrive HTML, css og javascript for at lave en hjemmeside.

Øvelsen formoder at du arbejder med et gitlab projekt som er klonet lokalt til din computer.

Der er temmelig mange instruktioner i denne øvelse og det kan virke mere uoverskueligt end det egentlig er, derfor har jeg har lavet et eksempel projekt så du har et sted at sammenligne med: [https://gitlab.com/npes/mkdocs-test](https://gitlab.com/npes/mkdocs-test)

Jeg vil også anbefale at du læser hele øvelsen igennem inden du går i gang med at arbejde, det vil hjælpe dig til at skabe et overblik.

_Dette er en individuel øvelse, men i skal arbejde sammen i par når i laver øvelserne. Undervejs skal i hjælpe hinanden så i når i mål med det hele._

### Instruktioner

1. Lav et projekt på gitlab.com i dit eget namespace `https://gitlab.com/bruger_navn`
2. Klon projektet til din computer med `git clone`, brug ssh til at klone projektet :-)
3. Installer Python og pip, følg mkdocs guiden, stop ved _Installing MKDocs_ [https://www.mkdocs.org/user-guide/installation/](https://www.mkdocs.org/user-guide/installation/)
4. Naviger til dit gitlab projekt, åben en terminal og lav et Python virtual environment [https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/#creating-a-virtual-environment](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/#creating-a-virtual-environment)
5. Aktiver virtual environment [https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/#activating-a-virtual-environment](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/#activating-a-virtual-environment)
6. Følg mkdocs getting started guiden [https://www.mkdocs.org/getting-started/](https://www.mkdocs.org/getting-started/) til og med punktet **Adding pages**
7. Flyt mappen `docs` og filen `mkdocs.yml` til en mappe i dit projekt, navngiv mappen `pages`
8. pip freeze gemmer dine ,virtual environment, package dependencies, så dit projekt senere kan genskabes uden en backup af selve pip pakkerne.  
   Læs om pip freeze [https://pip.pypa.io/en/stable/reference/pip_freeze/](https://pip.pypa.io/en/stable/reference/pip_freeze/) og lav en `requirements.txt` fil ved at køre kommandoen `pip freeze > requirements.txt`  
   Kontroller at `requirements.txt` indeholder dine dependencies, f.eks.
   ```
   text click==7.1.2
   future==0.18.2
   Jinja2==2.11.2
   joblib==0.17.0
   livereload==2.6.3
   lunr==0.5.8 Markdown==3.3.3
   MarkupSafe==1.1.1
   mkdocs==1.1.2
   nltk==3.5
   PyYAML==5.3.1
   regex==2020.11.13
   six==1.15.0
   tornado==6.1
   tqdm==4.53.0
   ```
9. .gitignore fil

   Det er god praksis ikke at gemme dependencies i dit git repository, de fylder unødvendigt og er ikke nødvendige for at genskabe projektet nu hvor du har din `requirements.txt`  
    Dependencies i dette projekt er dit virtuelle environment samt mkdocs site mappen.

   1. I roden af din gitlab projekt mappe, lav en fil der hedder `.gitignore`
   2. Åben `.gitignore` i en teksteditor
   3. Tilføj 2 seperate linjer i filen, `env/` og `site/` linjerne fortæller git at disse mapper skal ignoreres

10. Gitlab pipeline fil

    Gitlab bruger Gitlab runners til at køre docker containers [https://docs.gitlab.com/runner/](https://docs.gitlab.com/runner/)  
    Docker containere kører et OS image i en container.  
    Gitlab runner pipeline konfigurationen laver du i dit projekt, i en fil der hedder `.gitlab-ci.yml`  
    Filens sprog/syntaks hedder [yaml](https://en.wikipedia.org/wiki/YAML) og de kommandoer der eksekveres er `bash` kommandoer.

    Du kan læse mere om gitlab runners, pipelines og jobs her [https://docs.gitlab.com/ee/ci/pipelines/](https://docs.gitlab.com/ee/ci/pipelines/)

    I dit MKDocs projekt skal du konfigurer en pipeline som bruger en docker container med linux alpine som image.  
    I den container installeres et par ting, inklusiv MKDocs og herefter køres kommandoen `mkdocs build`  
    Endelig flyttes build outputtet (din mkdocs side) til gitlab projektets public mappe. Gitlab pages filern skal ligge i gitlab projektets public mappe, det er her gitlab server siden fra.

    1. I gitlab projektets rod lav en fil der hedder `.gitlab-ci.yml`
    2. Tilføj nedenstående til filen:

       ```yaml
       image: python:alpine
       before_script:
         - apk add --no-cache --virtual .build-deps gcc musl-dev
         - pip install cython
         - pip install --upgrade pip
         - pip install mkdocs
         # Add your custom theme if not inside a theme_dir
         # (https://github.com/mkdocs/mkdocs/wiki/MkDocs-Themes)
         # - pip install mkdocs-material
       pages:
         script:
           - cd pages
           - mkdocs build
           - mv site ../public
         artifacts:
           paths:
             - public
         only:
           - main
       ```

11. git add og commit nye filer

    1. Tilføj nye filer med `git add .`
    2. Commit med `git commit -am "added mkdocs"`
    3. Push med `git push`

12. Check din pipeline

    1. Naviger til projektet på gitlab.com
    2. Klik på `CI/CD` i menuen til venstre
    3. Klik på `pipelines`
    4. Klik på jobbet for at se hvad der sker
    5. Hvis jobbet er kørt uden fejl vil den sidste linje vise `Job succeeded`, hvis ikke så læs fejlbeskrivelsen og debug.

13. Kontroller at siden kører på gitlab

    1. På gitlab.com, i dit projekt, naviger til Settings > Pages og klik på linket under `Your pages are served under:`
    2. Første gang siden bygges kan der gå op til 30 minutter før siden er "live"

14. Skriv dit navn og link til din portfolio, i dette google dokument (login på google med ucl edu credentials) [https://docs.google.com/document/d/1eXK_6D_ECOgxvJSNGqqJQXjL-3mNMZg34ttUP_Pwzko/edit?usp=sharing](https://docs.google.com/document/d/1eXK_6D_ECOgxvJSNGqqJQXjL-3mNMZg34ttUP_Pwzko/edit?usp=sharing)

**_next steps_**

Brug MKDocs guide til at theme og udbygge din portfolio side [https://www.mkdocs.org/user-guide/](https://www.mkdocs.org/user-guide/)

## Øvelse 3 - gitlab pages til it-sikkerheds nyhedskilder

### Information

I denne øvelse skal i samarbejde om at lave en offentlig gitlab pages side til at liste de forskellige it-sikkerheds nyheds kilder i fandt i sidste uge.  
Igennem semestret skal i løbende opdatere siden med nye kilder i finder.  
Det vil også være kool hvis i kan lave kategorier så siden, udover nyhedskilder, har kategorier for it-sikkerheds tools og it-sikkerheds læringskilder.

Ideen er at i starter en tradition som kommende it-sikkerheds studerende kan bygge videre på.

Siden skal laves med MKDocs, som i øvelse 3.

Øvelsen giver jer også mulighed for at øve git branching, git merge og løsning af git merge conflicts.
Derfor er det ikke tilladt at pushe direkte til `main` branchen, alle kode integrationer skal lave med en `merge request` og alle conflicts skal `resolves` i branchen før ændringer merges.

I kan bruge gitlab docs som reference når i bruger git [https://docs.gitlab.com/ee/topics/git/](https://docs.gitlab.com/ee/topics/git/)  
gitlab har også lavet et git cheatsheet som kan være nyttigt [https://about.gitlab.com/images/press/git-cheat-sheet.pdf](https://about.gitlab.com/images/press/git-cheat-sheet.pdf)

Mens i laver øvelsen så brug projektoren i klassen, så alle kan følge med og byde ind mens i laver øvelsen.

### Instruktioner

**Del 1**

1. Beslut på klassen hvordan siden skal struktureres, hvilke informationer, underpunkter osv. skal der laves. Dokumentér strukturen i et `.md` dokument.
2. Hvilke informationer skal der være til hver nyheds kilde ?  
   Jeg foreslår en kort beskrivelse af kildens indhold, hvem der står bag kilden og selvfølgelig et link til kilden.
3. Lav et offentligt gitlab projekt, det skal navngives **ucl-itsec-student-ressources**.
4. Installer MKDocs som i øvelse 2, kontroller at gitlab pages køres.
5. Tilføj alle i klassen som `maintainer` og mig som `owner`, mit gitlab bruger navn er **npes**
6. Hver studerende _kloner_ projektet til egen computer
7. Hver studerende laver en branch i projektet og tilføjer 1 nyhedskilde.
8. Hver studerende laver en merge request og sætter en anden studerende til at reviewe og approve merge requesten, sørg for ikke at slette branchen når den merges til main.
9. Når alle har tilføjet en nyhedskilde via merge request er øvelsen gennemført.

**Hvis i laver git fejl, så se det som en god mulighed for at lære hvordan i kan rette fejlen.**  
**Brug [https://ohshitgit.com/](https://ohshitgit.com/) som reference og snak med hinanden om hvordan fejjlen bedst kan løses**

## Øvelse 4 - refleksions spørgsmål

### Information

Refleksions spørgsmål som du skal besvare i dine private noter.  
De kan bruges som forberedelse til fagets eksamen, men også som en hjælp til at reflektere over din viden.

### Instruktioner

Svar individuelt på følgende spørgsmål:

1. Hvorfor er det ikke sikkert at gemme sine private nøgler på andre computere? Hvad risikerer du ved det?
2. Er det sikkert at forwarde din private ssh nøgle med en user agent ? Uddyb dit svar.
3. Hvad kan du gøre for at sikre din gitlab konto yderligere ?
4. Er det bedre at arbejde i branches fremfor direkte på main branch ?
5. Hvad er fordelene ved at bruge git ?
6. Er det en fordel at arbejde med git fra CLI fremfor GUI ?
7. Hvor brugbare, for kommende studerende, er informationerne på nyheds siden og hvordan kan den forbedres ?
8. Hvad er yaml og hvor bruges det ?
9. Hvem har lavet git ?
10. Hvilke alternativer til gitlab findes der (research)
11. Hvordan kan en git merge conflict opstå og hvad kan du gøre for at løse den ?
12. Hvad er pip ?
13. Forklar fordelene ved at bruge et virtual environment i python
14. Skriv et kort resume af hvad du har lært i denne uges øvelser.
15. Hvilke it-sikkerheds nyheder gjorde størst indtryk på dig i denne uge ?
