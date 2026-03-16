Render:
- CSR: sem SEO, load
- SSR: server workload
- SSG
- ISR

RSC
- Hydration: interatividade componentes renderizam no server

Routes
- not-found
- error
- loading
- route (json)

Metadata
- const metadata
- generateMetadata()

icon.png

Loading
- loading.tsx
- Suspense fallback={loading}

Not Found:
- not-found.tsx
- notFound();

async function + 'use server'

useFormStatus

useActionState

revalidatePath('meals/')

---

Libs
- bettersqlite3
- slugify
