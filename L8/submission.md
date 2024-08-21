### Set up a collaborative translation tool.
we are using the Traduora which is collaborative translation tool.
Traduora is an Open-Source Translation Management Platform.
first of all inorder to use we need to follow the below steps:

```bash
git clone https://github.com/ever-co/ever-traduora
cd ever-traduora
docker-compose -f docker-compose.demo.yaml up![image](https://github.com/user-attachments/assets/2e889d01-d3ae-4b26-860f-995ffac37cf1)

```

Screenshots:
![image](https://github.com/user-attachments/assets/53b8a6cd-3586-459d-bf05-f3b4c48dfd48)
We can now access Traduora by visiting http://localhost:8080/

We need to create an account and then login using the crediantals and create a new project. Add all your terms in project then translate the terms into you prefered language
![image](https://github.com/user-attachments/assets/7238fbd8-a271-4c54-90fa-98ccf96b2bf6)

![image](https://github.com/user-attachments/assets/f9149469-7cf4-4b7c-82b2-5e54b2503d01)

![image](https://github.com/user-attachments/assets/67a1da18-5eb6-4d14-8ba0-abc03fc841b0)

We need to downlaod the translations in JSON format
![image](https://github.com/user-attachments/assets/03c97dd8-3e05-408c-bb44-3cea1384d9b4)

### Setting up your React project to make use of i18n.
#### Install Dependencies
i18next and react-i18next libraries are the core dependencies for setting up i18n in your React project:
To detect the user's language based on their browser settings, we need install the i18next-browser-languagedetector
```bash
npm install react-i18next i18next
npm install i18next-react-languagedetector
```
Create a local folder inside the src to store the translations
![image](https://github.com/user-attachments/assets/73f82d11-6d58-42e0-ae7c-8c918602b295)

Create a i18n.ts file in src folder to configure the i18next in our project:
and configure the i18next in the file
```ts
import i18n from "i18next";
import LanguageDetector from "i18next-browser-languagedetector";
import { initReactI18next } from "react-i18next";
import enJSON from "./locale/en.json";
import itJSON from "./locale/it.json";

i18n
  .use(LanguageDetector)
  .use(initReactI18next)
  .init({
    resources: {
      en: { ...enJSON },
      it: { ...itJSON },
    },
    fallbackLng: "en",
  });

export default i18n;

```

In the file we imported required dependices and file.
then i18n uses the language detector to know the language from browser settings.
intializes the resources which nothing but translation files and we are setting defualt langauge to english

and our translation files looks like
```json(it.json)
{
  "translation": {
    "American_Football": "Football americano",
    "Basketball": "Pallacanestro",
    "Commentary": "Commento",
    "Cricket": "Grillo",
    "Email": "E-mail",
    "Field_Hockey": "Hockey su prato",
    "Filter": "Filtro",
    "Go_Back": "Go Back",
    "Live_Matches": "Partite in diretta",
    "Login": "Accesso",
    "Match": "Fiammifero",
    "Match_Info": "Match Info",
    "Password": "Parola d’ordine",
    "Preferences": "Preferenze",
    "Profile": "Profilo",
    "Read_more": "Leggi di più",
    "Register": "Registro",
    "Rugby": "Rugby",
    "Scorecard": "Scorecard",
    "Sign_in": "Sign in",
    "Sign_out": "Sign out",
    "Story": "Storia",
    "Table_Tennis": "Ping-pong",
    "Teams": "Squadre",
    "Trending_Articles": "Articoli di tendenza",
    "Venue": "Sede",
    "Your_News": "Le tue notizie",
    "add": "Aggiungi preferenze per esperienze migliori",
    "error": "Accesso non riuscito: nome utente o password non validi",
    "msg": "Non hai un account? Crea un account.",
    "Date & Time": "Data e ora",
    "No_Articles ": "Nessun articolo correlato a",
    "load": "Caricamento...",
    "filter_sports": "Filtra in base agli sport",
    "Your_Name": "Il tuo nome",
    "acc": " Hai già un account? Accesso."
  }
}
```

We need to replace the hardcoded texts inside the application using translation.we import i18provider aand wrap it in App component so that we can access the i18n from anywhere inside the application
![image](https://github.com/user-attachments/assets/562edc3c-6a75-409a-a39f-97c160cf1e6a)

We can use translation anywhere in the application:

```tsx
import React, { Suspense } from "react";
import { useTranslation } from "react-i18next";
import ErrorBoundary from "../../components/ErrorBoundary";
const ArticlesList = React.lazy(() => import("./articles/ArticlesList"));
const MatchList = React.lazy(() => import("./matches/MatchList"));

const Home = () => {
  const { t } = useTranslation();
  return (
    <>
      <div className="flex justify-between text-center text-md-left px-5">
        <h2 className="text-2xl font-bold tracking-tight text-slate-700">
          {t("Live_Matches")}
        </h2>
      </div>
      <ErrorBoundary>
        <Suspense fallback={<div className="suspense-loading">{t('load')}</div>}>
          <MatchList />
        </Suspense>
      </ErrorBoundary>

      <div className="flex justify-between text-center text-md-left px-5">
        <h2 className=" text-2xl font-bold tracking-tight text-slate-700">
          {t("Trending_Articles")}
        </h2>
      </div>
      <div className="flex">
        <ErrorBoundary>
          <Suspense
            fallback={<div className="suspense-loading">{t('load')}</div>}
          >
            <ArticlesList />
          </Suspense>
        </ErrorBoundary>
      </div>
    </>
  );
};

export default Home;

```

English:
![image](https://github.com/user-attachments/assets/a8a4dfca-d375-40bc-ae84-cf9c320974a3)

Italian:
![image](https://github.com/user-attachments/assets/1b3fd226-f5b6-4e22-8054-76c104b549bd)


### Setting up an automatic language detector and switch the locale according to it.
The langauge Detector automatically detects the language from user browser configuartions and  Changes the locale accordingly.





