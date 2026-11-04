# Kenneth Minkinen

Product engineer: React Native, React, Node.

I build mobile apps and the servers behind them. Two are live on the App Store and Google Play: [Tsemppi](https://apps.apple.com/fi/app/id6757228735?l=en), a workout app I co-founded and lead development on, and [Ravinne](https://apps.apple.com/fi/app/id6744070817?l=en), a nutrition tracker I built alone. Most days that means React Native with Expo on the phone and Node with Express and MongoDB on the server. When a widget or a watch app needs it, I write the Swift and Kotlin too.

Based in Helsinki. Open to work in Estonia, Finland or remote in the EU, available immediately. I'm an EU citizen (half-Estonian, half-Finnish), so there is no visa or sponsorship to sort out. I'm looking for a first team job as a junior, mid or product engineer. The quickest way to reach me is [LinkedIn](https://www.linkedin.com/in/kenneth-minkinen-118643281/).

I build with AI coding tools. I have no years in a team or under code review yet, and that is what I want next.

## Tsemppi

A strength training app for iPhone, Android, Apple Watch and Wear OS. **It has more than 4,000 registered users and reached #1 on the Finnish App Store's Health & Fitness chart for a few days in autumn 2026.** About 900 people opened it in the 28 days to 28 October 2026 and logged 30,888 sets. It is rated 4.18 from 34 ratings on the Finnish App Store (4 November 2026).

We are three founders and I lead development. In the app repo, 456 of the 459 commits on the release line in the stores are mine: the React Native app, the Express API, and the native widgets and watch apps. I also built the admin dashboard in our Next.js website, where we answer support chats in real time.

- 293 API route handlers and 36 MongoDB models. The 29 scheduled jobs run in their own worker process under PM2, apart from the API.
- Native code: 21k lines of Swift for the iOS widgets, Live Activities, notification extension and watchOS app, and 21k lines of Kotlin, mostly the Wear OS app.
- The interface ships in Finnish, English, Swedish, Danish and Norwegian, 10,610 strings each.
- 1,247 test files with about 18,900 cases. The full suite runs before every commit and again in CI.

[App Store](https://apps.apple.com/fi/app/id6757228735?l=en) | [Google Play](https://play.google.com/store/apps/details?id=com.tsemppi.app) | [tsemppiapp.fi](https://tsemppiapp.fi) | [Case study](https://github.com/KennethMin/tsemppi-case-study)

## Ravinne

A calorie and nutrition tracker for iPhone and Android. I built all of it alone, from the app and the Node API to the admin dashboard and the website: all 772 commits in the app repo up to the last store build are mine. **It has been on the App Store since November 2025, and version 2.2.0, a rebuild of most of the app, went live on 30 October 2026.** It is rated 4.05 from 19 ratings in Finland (4 November 2026), with 500+ downloads on Google Play.

You can log a meal by scanning a barcode, taking a photo, or saying or typing a sentence. 44,666 foods ship inside the app, so a scan can answer with no network. The weekly calorie target comes from a deterministic regression over the food log and the weight trend, and no language model touches it. The website, [ravinne.fi](https://ravinne.fi), is Next.js 16 and TypeScript.

[App Store](https://apps.apple.com/fi/app/id6744070817?l=en) | [Google Play](https://play.google.com/store/apps/details?id=com.kende6644.StickerSmash) | [ravinne.fi](https://ravinne.fi) | [Case study](https://github.com/KennethMin/ravinne-case-study)

## Engineering notes

- Tsemppi used to score workout routines with an LLM, from 1 to 10, and asking twice could give the same routine two different scores. I replaced it with a pure function, shared by the app and the server, that counts effective weekly sets per muscle. The model now only writes a note, and the server drops the note if it quotes a number the function didn't compute.
- Ravinne's old weekly estimate compared the start and end of one week, which the normal weekly rhythm of body weight biases. In simulation about 70% of its adjustments against a true 100 kcal gap pointed the wrong way. It now fits a regression over a window of up to 42 days.
- Photo and voice results are matched against the food database: a good match replaces the model's recalled nutrition with a real row and a household portion, and a weak one is only offered as an alternative.
- OpenAI announced shutdowns for four of the models Ravinne's server calls within eleven weeks. Their ids sat in fifteen call sites. Now every model is a role in one file, with a default that switches to its named replacement on the shutdown date.
- Raw phone GPS overstates run distance by 5-15%. Tsemppi's run recorder only counts a new point once it is more than twice its reported accuracy away from the last one, because two readings taken while you wait at a crossing can differ by that much. The filter is tested against synthetic traces, so no device is needed.
- A training plan can be edited with one sentence. The model sees rows only as refs (r1 to rN) and returns up to 8 intents, checked against a strict JSON schema. The app applies them through the plan screen's own edit operations, with undo, so the model never writes a row.
- Quiet hours hold a deferrable push until the window ends and drop the time-bound ones. After a cron job once sent a push to the whole user base, I added a daily cap on distinct recipients.
- Exercise search folded accents with Unicode decomposition, which has none for ø and æ, so it deleted them. Mapping them by hand took the Danish query "loft" (for "løft") from 0 to 43 results.

## Other work

- [maxehitus.ee](https://maxehitus.ee) is the Estonian-language site of a construction company in Rakvere: 44 pages in Next.js 16, with a price calculator. Three redirects once turned out to 404 in production, so I added a post-deploy script that checks every route and redirect against the live site.
- kapahoid.ee is the site of a family-run dog hotel in Rakvere, also in Estonian. Its booking calculator handles weekend and holiday surcharges and a weekly discount, which is why 7 nights can cost less than 6.

The source for all of this is private. The case studies explain how the two apps work.

Tools I use most: React Native with Expo, Next.js, Node with Express, MongoDB, Jest, Sentry, PostHog and RevenueCat. The two apps are JavaScript. ravinne.fi is TypeScript.
