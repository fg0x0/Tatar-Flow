# Tatar Flow

> **Concept only — there is no code in this repository yet.**
> This repo holds a design document that is meant to be argued with and sharpened.

**Burp sees requests. Tatar Flow sees business functions.**

[**→ Read the concept document**](docs/concept.html) *(open the file in a browser; it is a
self-contained page with no dependencies)*

---

## The idea in one paragraph

A digital bank is not one web application; it is forty-odd microservices.
`POST /api/v1/transfer` fans out behind the scenes into limit-check, fee-calc,
risk-engine and ledger. Burp's site map shows URLs and methods, so all of that
structure lives only in the tester's head: which request is which business function,
where `txnId` came from, which step must precede which, and which actor may touch
which object. Tatar Flow turns captured traffic into a **semantic model** —
function, state, actor, object, variable — and uses it to hand the tester
context-filled valid requests, an organised catalogue of business-logic mutations
(actor swap, object swap, step skip, replay, value tamper, function confusion), and a
coverage view of what has actually been tested.

It is explicitly **not** an OpenAPI-to-request generator; that problem is solved. The
wedge is the layer above: modelling the application's business behaviour and managing
test context and coverage from one workspace.

## Status

| | |
|---|---|
| **Stage** | Concept / design draft |
| **Code** | None yet |
| **Document** | [`docs/concept.html`](docs/concept.html) |
| **Intended input** | Burp history / HAR (optionally OpenAPI), via mitmproxy or a Burp extension |
| **Licence** | MIT |

The document's roadmap sketches v0.1 (function workspace) → v0.2 (workflow/state) →
v0.3 (test intents) → v0.4 (offline Ollama assistance) → v1.0 (engagement coverage
and evidence).

## Scope and authorisation

Tatar Flow is **security testing tooling**, and the document is deliberate that it must
not become an automatic exploit scanner. Its value is: understand → model → organise
state → produce a valid test context → track coverage → **a human confirms**. It
proposes mutations; it does not fire them off on its own.

Use only within an engagement you have **written authorisation** for.

## Composes with Tatar Relay

Flow decides *which business function to test and with what context*. [Tatar
Relay](https://github.com/ochmunkh/Tatar-Relay) decides *how to speak the target's
encrypted or signed protocol*. Flow is the business envelope, Relay is the crypto
envelope; they are designed to be used together, and the concept document's data-flow
diagram shows the hand-off.

## Where this sits

Each Tatar tool answers one question:

| | You already have | Tatar tells you |
|---|---|---|
| Tatar-Triage | Logs on a host | what to look at |
| Tatar-Kuber | Findings from K8s scanners | what to trust |
| Tatar-Path | Attack paths from BloodHound | where to start |
| **Tatar-Flow** | **HTTP requests in a proxy** | **what to test, in what context** |
| Tatar-Relay | An encrypted/signed protocol | how to talk to it |

## Feedback

The document is a draft meant to be sharpened by discussion. The most useful
disagreement is about the semantic model itself — whether function, state, actor,
object and variable are the right five things to model, and what is missing.

---

<h2 id="mongol">Монгол хэл дээр</h2>

> **Зөвхөн концепц — энэ репод код хараахан байхгүй.**
> Энд байгаа нь хэлэлцэн хурцлах зорилготой дизайн ноорог.

**Burp request хардаг. Tatar Flow бизнес функц хардаг.**

[**→ Концепцийн баримтыг унших**](docs/concept.html) *(хөтчөөр нээнэ — гадны хамааралгүй,
бүтэн нэг хуудас)*

### Санаа нэг догол мөрөөр

Дижитал банк гэдэг нэг вэб апп биш, 40 гаруй микросервисийн нийлбэр.
`POST /api/v1/transfer` нь цаанаа limit-check, fee-calc, risk-engine, ledger рүү
задардаг. Burp-ийн site map зөвхөн URL, method-ыг харуулдаг тул энэ бүх бүтэц
тестерийн толгойд л үлддэг: аль request нь ямар бизнес функц вэ, `txnId` хаанаас
ирсэн бэ, аль алхам аль алхмынхаа өмнө байх ёстой вэ, аль актор аль объектод хүрч
болох вэ. Tatar Flow нь цуглуулсан трафикийг **семантик загвар** — функц, төлөв,
актор, объект, хувьсагч — болгон хувиргаж, тестерт контекстээр дүүргэгдсэн хүчинтэй
request, бизнес логикийн mutation-уудын зохион байгуулалттай каталог (актор солих,
объект солих, алхам алгасах, replay, утга гажуудуулах, функц андуурах), мөн юу
бодитоор тестлэгдсэн тухай хамралтын дүр зургийг өгнө.

Энэ нь OpenAPI-аас request үүсгэгч **биш** — тэр асуудал аль хэдийн шийдэгдсэн.
Шаантаг нь дээд давхаргад: аппликешны бизнес зан төлөвийг загварчилж, тестийн контекст
ба хамралтыг нэг workspace-ээс удирдахад оршино.

### Төлөв

| | |
|---|---|
| **Үе шат** | Концепц / дизайн ноорог |
| **Код** | Хараахан байхгүй |
| **Баримт** | [`docs/concept.html`](docs/concept.html) |
| **Төлөвлөж буй оролт** | Burp history / HAR (сонголтоор OpenAPI), mitmproxy эсвэл Burp өргөтгөлөөр |
| **Лиценз** | MIT |

Баримтын замын зураг: v0.1 (функцийн workspace) → v0.2 (workflow/төлөв) → v0.3 (тестийн
санаанууд) → v0.4 (офлайн Ollama туслалцаа) → v1.0 (engagement-ийн хамралт ба нотолгоо).

### Хүрээ ба зөвшөөрөл

Tatar Flow бол **аюулгүй байдлын туршилтын хэрэгсэл** бөгөөд баримт нь үүнийг автомат
exploit scanner болгож болохгүй гэдгийг тодорхой заасан. Үнэ цэнэ нь: ойлгох →
загварчлах → төлөв зохион байгуулах → хүчинтэй тестийн контекст үүсгэх → хамралт хянах
→ **хүн баталгаажуулах**. Mutation-ыг санал болгоно; өөрөө довтлохгүй.

Зөвхөн **бичгээр зөвшөөрөгдсөн** engagement-ийн хүрээнд хэрэглэнэ.

### Tatar Relay-тэй хамт ажиллана

Flow нь *ямар бизнес функцийг ямар контекстээр тестлэхийг* шийднэ. [Tatar
Relay](https://github.com/ochmunkh/Tatar-Relay) нь *зорилтын шифрлэгдсэн эсвэл гарын
үсэгтэй протоколыг хэрхэн ярихыг* шийднэ. Flow бол бизнес дугтуй, Relay бол крипто
дугтуй; хоёулаа цуг ажиллахаар зохиогдсон бөгөөд концепцийн баримтын дата урсгалын
зураглал энэ шилжилтийг харуулна.

### Экосистем дэх байрлал

Tatar хэрэгсэл бүр нэг асуултад хариулна:

| | Чамд бэлэн байгаа | Tatar хэлж өгнө |
|---|---|---|
| Tatar-Triage | Хост дээрх лог | юу харах вэ |
| Tatar-Kuber | K8s scanner-ийн finding | юунд итгэх вэ |
| Tatar-Path | BloodHound-ийн attack path | алинаас эхлэх вэ |
| **Tatar-Flow** | **Proxy дээрх HTTP request** | **ямар функцийг ямар контекстээр тестлэх вэ** |
| Tatar-Relay | Шифрлэгдсэн/гарын үсэгтэй протокол | хэрхэн харилцах вэ |

### Санал хүсэлт

Баримт бол хэлэлцэн хурцлах зорилготой ноорог. Хамгийн хэрэгтэй эсрэг санал бол
семантик загвар өөрөө — функц, төлөв, актор, объект, хувьсагч гэсэн таван зүйл зөв
сонголт мөн эсэх, юу дутуу байна вэ гэдэг.
