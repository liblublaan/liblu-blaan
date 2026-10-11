# Liblu Blaan

Isang digital na tahanan para sa wikang Blaan, kultura, tradisyon, at kaalamang pamana.

## Mga file sa website

Panatilihin ang mga sumusunod na file sa **root folder** ng GitHub repository (sa parehong antas):

- `index.html` — pangunahing website code.
- `liblu-blaan-logo.svg` — logo na ginagamit sa header at footer.
- `83a71bfc-8ba3-4c24-becc-a9369d8d1897.png` — bagong 16:9 landscape cultural banner.

Huwag palitan ang pangalan ng banner file maliban kung ia-update rin ang `src` nito sa `index.html`.

## Mga update sa bersiyong ito

- Idinagdag ang LIBLU BLAAN landscape banner na may 16:9 aspect ratio para sa website at social media cover.
- Idinagdag ang logo asset.
- Inayos ang pagpapakita ng mga post na may kategoryang `Dictionary` sa Dictionary section.
- Idinagdag ang rich-text editing tools sa Writing Lounge para sa basic formatting gaya ng bold, italic, underline, headings, lists, alignment, at spacing.

## Pag-publish sa GitHub Pages

1. Gumawa ng backup ng kasalukuyang `index.html` at `README.md` bago magpalit ng files.
2. I-upload o palitan ang `index.html`, `liblu-blaan-logo.svg`, at `83a71bfc-8ba3-4c24-becc-a9369d8d1897.png` sa root folder ng repository.
3. Palitan ang `README.md` ng file na ito kung nais mong i-update ang dokumentasyon.
4. Sa GitHub, ilagay ang commit message:

   `feat: add Liblu Blaan banner and improve writing lounge`

5. I-commit ang mga pagbabago at hintaying matapos ang GitHub Pages deployment.
6. Buksan ang live website at subukan ang banner, logo, Dictionary section, Writing Lounge, at login. Kung may problema, tingnan ang browser console at Supabase logs.

## Supabase at seguridad

- Gamitin lamang ang Supabase publishable/anon key sa browser-side code.
- **Huwag kailanman ilagay** ang `service_role` key o anumang secret key sa `index.html`, README, o GitHub repository.
- Tiyaking tama at mahigpit ang Row Level Security (RLS) policies para sa mga table na ginagamit ng website.
- Ang pag-upload ng bagong HTML ay hindi awtomatikong naglilipat ng mga post na naka-save lamang sa browser `localStorage` papunta sa Supabase. Gumamit ng umiiral na import/migration workflow kung kinakailangan.

## Mga tala sa pagsubok

Na-check ang JavaScript syntax at duplicate HTML IDs sa inihandang HTML. Hindi ito katumbas ng buong end-to-end test; kailangan pa ring subukan ang aktuwal na GitHub Pages deployment, login, at Supabase operations.
