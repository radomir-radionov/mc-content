# mc-content — MyConspect study notes

Public markdown conspect for [MyConspect](https://myconspect.vercel.app): bilingual study notes in **English** and **Russian**.

| Directory | Locale |
| --------- | ------ |
| `en/`     | English |
| `ru/`     | Russian (same basenames as `en/`) |

The [mc-app](https://github.com/radomir-radionov/mc-app) Next.js app renders topics listed in its catalog. Files here that are not in the catalog (for example personal link hubs) stay out of the site but remain in this repository.

## Clone

```bash
git clone https://github.com/radomir-radionov/mc-content.git
```

## Dev workspace (with mc-app)

From the [mc-agents](https://github.com/radomir-radionov/mc-agents) layout:

```bash
cd mc-agents
git clone https://github.com/radomir-radionov/mc-content.git
git clone -b dev https://github.com/radomir-radionov/mc-app.git
cp mc-app/.env.example mc-app/.env.local   # MYCONSPECT_NOTES_ROOT=../mc-content
cd mc-app && npm install && npm run dev
```

Push **mc-content** before **mc-app** when both change so deploys pick up fresh markdown.

## CI / Vercel

mc-app’s `prebuild` runs `scripts/ensure-resources.ts`, which shallow-clones this repo when `resources/` is empty. Set `RESOURCES_GIT_URL` to this repository’s HTTPS URL (see mc-app README). A token is optional for a **public** repo.

## License

[Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE).
