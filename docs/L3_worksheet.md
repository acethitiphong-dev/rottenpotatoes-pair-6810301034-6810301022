# ใบงาน Lab L3 จากคำของลูกค้าสู่ scenario ที่รันได้

ใบงานนี้มีทุกโจทย์ของ Lab L3 และช่องที่ต้องเติม ช่องที่มีเฉลยในสไลด์ถัดไปเขียนเป็น `[[เติม n.k: ...]]` ส่วน `<A>`, `<B>` และ `<issue>`
คือชื่อของคู่และเลข Issue จริงของคู่ ไม่มีเฉลย ทุกกล่องโค้ดในใบงานใช้ช่องว่างธรรมดา คัดลอกจากใบงานได้ อย่าคัดลอกโค้ดจากสไลด์
บรรทัดที่อยู่ระหว่าง `<!-- block ... -->` กับกล่องโค้ดถัดไปคือเนื้อหาของไฟล์ที่ระบุ คัดลอกเฉพาะข้อความในกล่อง

## 0 ก่อนเริ่ม

- [✅ ] JDK 25 และ main ของคู่ผ่าน `./mvnw -B verify` ด้วย Tests run: 15 บนทั้งสองเครื่อง
- [✅] badge ของ L2 ใน README ของ repository ของคู่เป็น passing
- [✅] A และ B เป็น collaborator ของ repository ของคู่ และรับคำเชิญแล้ว
- [✅] รู้ทีมและ product owner ของ Iteration 0 จาก Moodle
- [✅] แตก L3_lab-kit.zip ไว้ที่ `~/Downloads/L3_lab-kit` บนทั้งสองเครื่อง
- [✅] ผู้ช่วย AI ที่สถาบันอนุญาตเข้าสู่ระบบแล้วบนเครื่อง A หรือจะใช้ร่างใน kit
- [✅] บัตร A7 3 ใบ กระดาษ A4 2 แผ่น ปากกา โทรศัพท์ อ่าน Brief_SE.pdf และ Pressman บทที่ 7 มาแล้ว

0.1 ไฟล์ `src/test/resources/junit-platform.properties` (สร้างในขั้นเตรียม)

<!-- block 0.1 src/test/resources/junit-platform.properties -->
```properties
cucumber.glue=th.ac.cdti.rottenpotatoes.acceptance
cucumber.plugin=pretty
cucumber.publish.quiet=true
```

## 1 โจทย์ 1: intent จากคำของลูกค้า

คำของลูกค้า (customer proxy คือผู้ปกครองของเด็ก 8 ขวบ ตัวอย่างของรายวิชา):
"เวลาจะเลือกหนังให้ลูก 8 ขวบ ผมต้องเปิดดูทีละเรื่องว่า rating อะไร รายการยาวมาก อยากหาหนังที่ลูกดูได้เร็วกว่านี้"

| คำถามของคู่ | คำตอบของ customer proxy |
|---|---|
| ลูกดูได้ คือ rating ใด | แล้วแต่เรื่อง ผมอยากเลือก rating เอง |
| เร็ว หมายถึงอะไร | ไม่ต้องเปิดดูทีละเรื่อง |
| ต้อง login มีโปรไฟล์อายุ หรือให้ระบบแนะนำหนังไหม | ไม่ต้อง และไม่อยากให้เก็บข้อมูลส่วนตัว |
| มีอย่างอื่นที่อยากได้อีกไหม | อยากเห็นหนังใหม่ก่อน |
| ถ้าไม่มีหนัง rating นั้นเลย จะให้เห็นอะไร | ยังไม่ได้คิด |
| เลือกหลาย rating พร้อมกันได้ไหม | ยังไม่แน่ใจ |

คัดลอกกล่องล่างเป็น `docs/intent/movie-filter.md` แทน `<A>` และ `<B>` ด้วยชื่อของคู่ แล้วเติมทั้ง 5 ช่องเป็นภาษาอังกฤษ
ช่องละหนึ่งหรือสองบรรทัด ห้ามเปิดผู้ช่วยระหว่างเขียน หัวข้อ Affected users and systems และบรรทัดสุดท้ายของ Constraints ให้ไว้เป็นตัวอย่าง
ห้าหัวข้อนี้ตามแบบ intent.md ของ Anthropic (2026) หน้า 13 (แนวปฏิบัติของ Anthropic)

<!-- block 1 docs/intent/movie-filter.md -->
```markdown
# Intent: [[เติม 1.1: สิ่งที่ผู้ใช้อยากทำได้ ในหนึ่งบรรทัด]]
Author: <A> and <B>, for the customer proxy (a parent of an 8-year-old). Status: draft.

## Problem
[[เติม 1.2: สิ่งที่ทำไม่ได้หรือช้าในวันนี้ ตามคำของลูกค้า สองบรรทัด]]

## Proposed outcome
[[เติม 1.3: เมื่อเสร็จแล้วผู้ใช้ทำอะไรได้ ไม่เขียนชื่อ class สองบรรทัด]]

## Affected users and systems
Parents first, then every moviegoer. The movie catalog of Rotten Potatoes (Movie, Rating and
MovieCatalog from L2). There is no web page yet: the behaviour is checked through the domain classes.

## Constraints
[[เติม 1.4: กติกาและข้อมูลที่ต้องจริงเสมอ จากคำตอบของลูกค้า หนึ่งบรรทัด]]
Plain Java this week, no database.

## Open questions
[[เติม 1.5: สิ่งที่ลูกค้ายังไม่ตัดสิน บรรทัดละคำถาม]]
```

## 2 โจทย์ 2: ให้ผู้ช่วยร่าง แล้วตรวจทุกบรรทัด

2.1 สร้าง `docs/ai/l3-prompt.txt` วางกล่องล่างทั้งกล่อง ขึ้นบรรทัดใหม่ แล้ววาง intent ทั้งไฟล์ต่อท้าย จากนั้นวางทั้งไฟล์ในแชตใหม่ของผู้ช่วยและส่งครั้งเดียว
prompt นี้ดัดแปลงจากตัวอย่างของ Anthropic (2026) หน้า 16 (แนวปฏิบัติของ Anthropic)

<!-- block 2.1 docs/ai/l3-prompt.txt (first 10 lines) -->
```text
Read the intent.md below and write a requirements spec for it as spec.md.

1. List the functional requirements as numbered statements (REQ-1, REQ-2, ...). Each one must be testable.
2. Write 3 user stories in the Connextra form (As a ..., So that ..., I want ...), each with acceptance criteria.
3. For the first story, write Gherkin scenarios: one happy path and at least one sad path.
4. List every assumption you made that is not in the intent, and every area of concern.

Do not write code. Keep to what the intent asks for.

--- intent.md ---
```

2.2 สร้าง `docs/ai/l3-draft.md` พิมพ์สามบรรทัดแรกตามกล่องล่าง แล้ววางคำตอบทั้งหมดของผู้ช่วยแทนบรรทัดสุดท้าย ตามที่ได้ ไม่แก้แม้แต่บรรทัดแรก
คู่ที่ไม่มีผู้ช่วยยังสร้าง `docs/ai/l3-prompt.txt` ตามข้อ 2.1 แล้วคัดลอก `l3-draft_sample.md` ของ kit ทั้งไฟล์ (คำสั่งในสไลด์โจทย์ 2)
ร่างนั้นตอบ intent ตัวอย่างของรายวิชาในเฉลยโจทย์ 1 บรรทัด `Prompt: kit s19 prompt + kit s17 intent` ในหัวไฟล์ใช้ชื่อไฟล์ของ kit ฉบับก่อน
หมายถึง prompt ข้อ 2.1 กับ intent ของเฉลยโจทย์ 1 ไม่ต้องแก้ เพราะร่างต้องเก็บตามที่ได้ทุกตัวอักษร

<!-- block 2.2 docs/ai/l3-draft.md (header) -->
```text
Tool: <ชื่อและรุ่นของผู้ช่วยตามที่หน้าจอแสดง>
Date: <วันที่ เช่น 2026-11-26>
Prompt: docs/ai/l3-prompt.txt

<วางคำตอบทั้งหมดของผู้ช่วยตรงนี้ ตามที่ได้ ไม่แก้>
```

2.3 อ่านร่างทีละบรรทัดด้วยสี่คำถาม แล้วเติมตารางในกล่องล่างเป็น `docs/ai/l3-review.md` อย่างน้อย 3 แถว (คัดลอกแถวตัวอย่างเพิ่มได้)
คู่เขียนเองทุกแถว ผู้ช่วยไม่เติมตารางนี้ เพราะการตรวจคือการตัดสินของคน
1. intent หรือลูกค้าขอสิ่งนี้หรือไม่ ถ้าไม่ขอ ตัด
2. ร่างตอบ Open questions แทนลูกค้าหรือไม่ ถ้าใช่ แก้เป็นคำถามถึง customer proxy
3. ผู้ใช้เห็นและทดสอบได้หรือไม่ คำอย่าง exception, per call, deterministic คือคำของโค้ด
4. เป็น story ใหม่จริง หรือเป็นเงื่อนไขของ story อื่น เช่น sad path

<!-- block 2.3 docs/ai/l3-review.md -->
```markdown
# Review of docs/ai/l3-draft.md
Reviewers: <A> and <B>. Checked against docs/intent/movie-filter.md and SMART.

| บรรทัดในร่าง | ปัญหา | ตัดสิน: แก้ ตัด หรือเก็บ |
|---|---|---|
| [[เติม 2.1: เลขบรรทัดและข้อความที่ยกจากร่าง]] | [[เติม 2.2: ผิดอย่างไร หรือถูกเพราะอะไร]] | [[เติม 2.3: แก้เป็นอะไร หรือตัด หรือเก็บ]] |

Kept: [[เติม 2.4: สิ่งที่ร่างทำถูกหนึ่งข้อ และเพราะอะไร]]
Counts: แก้ [[เติม 2.5]] ตัด [[เติม 2.5]] เก็บ [[เติม 2.5]]
Customer proxy: <ใครยืนยันบัตรทั้งสามใบ เมื่อใด>
```

## 3 โจทย์ 3: บัตร story ที่ลูกค้ายืนยัน

เขียนบนบัตร A7 เป็นภาษาอังกฤษ ทั้งสามบรรทัด Connextra ต้องมี

```text
บัตร 1: Filter movies by rating
As a concerned parent
So that I can quickly browse movies appropriate for my family
I want to see only movies with a chosen rating
Happy: the parent picks one rating and sees only the movies with that rating

บัตร 2: Sort movies
As a moviegoer
So that I can find new releases and browse the catalog by name
I want the movie list sorted newest first or alphabetically

บัตร 3: Sad path ของบัตร 1
When no movie has the chosen rating,
the parent sees no movies and the message "No movies with this rating"
```

ตรวจแต่ละใบด้วย SMART ก่อนยื่นให้ customer proxy (Fox & Patterson, 2021, หน้า 209 ถึง 210)

| ตัวอักษร | คำถามที่ navigator ถาม | ตัวอย่างจากตำรา ไม่ผ่าน กับ ผ่าน |
|---|---|---|
| Specific | อ่านแล้วรู้หรือไม่ว่าเสร็จคืออะไร | User can search for a movie กับ User can search for a movie by title |
| Measurable | มี input ที่รู้และผลที่คาดไว้หรือไม่ ใน scenario คือ Given ตั้งสภาพเริ่มต้น และ Then ตรวจผลที่คาด | should have good response time กับ should appear within 3 seconds |
| Achievable | ทำเสร็จใน iteration เดียวหรือไม่ ถ้าไม่ ให้แบ่ง | story ที่ต้องทำหลายหน้าจอ ให้แยกเป็นหลายใบ |
| Relevant | ถามว่า ทำไม ซ้ำจนถึงคุณค่าทางธุรกิจของ stakeholder | Five Whys จากตำรา: ขายตั๋วได้มากขึ้น โรงละครอยู่รอด |
| Timeboxed | ถ้าเกินงบเวลาจะหยุด แบ่ง หรือประเมินใหม่ ไม่ทำต่อเงียบ ๆ | ใบที่กินเวลาเกินจะถูกแบ่งใน Iteration ถัดไป |

คำถามที่ต้องถาม customer proxy เมื่อยื่นบัตร: สอง Open questions ของ intent และความหมายของคำว่า newest
เมื่อ customer proxy ยืนยันแล้ว A วางสามบรรทัด Connextra ของใบที่ 1 ในคำอธิบายของ Issue

## 4 โจทย์ 4: Lo-Fi ของบัตรใบที่ 1

- [ ] หน้าจอ 1: [[เติม 4.1: สิ่งที่ผู้ใช้เห็นก่อนเลือก rating]]
- [ ] หน้าจอ 2: [[เติม 4.2: หลังเลือก PG-13]]
- [ ] หน้าจอ 3 (sad path): [[เติม 4.3: หลังเลือก G ซึ่งไม่มีหนัง]]
- [ ] ขีดเส้นใต้ทุกสิ่งที่ผู้ใช้กดได้ (ลูกศรจากสิ่งที่กดไปหน้าจอที่ปรากฏเป็นงาน storyboard นอกคาบ ข้อ 10)
- [ ] ถ่ายภาพ 1 ภาพให้เห็นทั้งสามหน้าจอ เก็บไว้เป็น `docs/lofi/rp-filter.jpg` (ส่งนอกคาบ)

## 5 โจทย์ 5: scenario ของบัตรใบที่ 1 แล้ว red

เติมทุกบรรทัด `# TODO 5.k` ใน `movie_filter.feature` ของ kit (บรรทัด Background ให้ไว้เป็นตัวอย่างถ้อยคำ) แล้วเทียบกับ Gherkin
ของร่างในบรรทัด 45 ถึง 68 ของ `l3-draft_sample.md` ว่าการตรวจในโจทย์ 2 เปลี่ยนอะไร

คำสำคัญ: Feature คือชื่อและคำอธิบาย, Background คือ step ที่รันก่อนทุก scenario, Scenario คือตัวอย่างรูปธรรมหนึ่งเรื่อง
(แนะนำ 3 ถึง 5 step), Given คือบริบทเริ่มต้น, When คือการกระทำ, Then คือผลที่สังเกตได้จากภายนอก, And และ But ต่อจาก step ก่อนหน้า

สร้าง package `acceptance` ใต้ `src/test/java/th/ac/cdti/rottenpotatoes` แล้วสร้าง class จากกล่องล่าง (ทั้งไฟล์)

<!-- block 5 src/test/java/th/ac/cdti/rottenpotatoes/acceptance/RunCucumberTest.java -->
```java
package th.ac.cdti.rottenpotatoes.acceptance;

import org.junit.platform.suite.api.IncludeEngines;
import org.junit.platform.suite.api.SelectPackages;
import org.junit.platform.suite.api.Suite;

/** Runs every .feature under src/test/resources/features through Cucumber on the JUnit Platform. */
@Suite
@IncludeEngines("cucumber")
@SelectPackages("features")
public class RunCucumberTest {}
```

ก่อนรัน `./mvnw -B test` จดคำทาย:
- step แรกของแต่ละ scenario จะมีสถานะ [[เติม 5.6]] และ step ที่เหลือจะมีสถานะ [[เติม 5.6]]
- Maven จะสรุปว่า Tests run: [[เติม 5.7]] Errors: [[เติม 5.7]] เพราะ [[เติม 5.7]]

## 6 โจทย์ 6: step definitions ให้ green

เติมทุกบรรทัด `throw new io.cucumber.java.PendingException("TODO 6.k ...")` ใน `MovieSteps.java` ของ kit
Given เพิ่มหนังลงใน `movies`, When เก็บชื่อของหนังที่ได้ไว้ใน `shown`, Then ตรวจ `shown` ด้วย AssertJ

| ใช้ได้จาก L2 และ AssertJ | ทำอะไร |
|---|---|
| `new Movie(title, rating, description, releaseDate)` | สร้างหนังหนึ่งเรื่อง description ว่างได้ |
| `Rating.fromLabel("PG-13")` | แปลงข้อความบนโปสเตอร์เป็น Rating |
| `LocalDate.of(year, 1, 1)` | วันที่ 1 มกราคมของปีที่ออกฉาย |
| `new MovieCatalog(movies).byRating(rating)` | หนังที่มี rating นั้น เรียงตามชื่อ |
| `.stream().map(Movie::title).toList()` | เหลือเฉพาะชื่อ |
| `assertThat(shown).contains(t)`, `.doesNotContain(t)`, `.isEmpty()` | ตรวจรายชื่อ |

## 7 โจทย์ 7: บัตรใบที่ 2 โดย B

B สร้าง `src/test/resources/features/movie_sort.feature` เองจากบัตรใบที่ 2 ที่ customer proxy ยืนยัน: scenario happy 1 เรื่อง
(หนัง 3 เรื่อง เรียงหนังใหม่ก่อน) และ sad 1 เรื่อง (catalog ว่าง) ไม่มี Background ใช้ถ้อยคำ step ที่มีอยู่แล้วให้มากที่สุด:

```text
a movie {string} rated {string} released in {int} is in the catalog
I filter movies by rating {string}
I should see {string}
I should not see {string}
I should see no movies
```

รันจนแดง commit red แล้วเขียน step ที่ยังขาดใน `MovieSteps.java` ชื่อ method แบบ camelCase เช่น `theCatalogIsEmpty`
เพิ่มบรรทัดว่างก่อนแต่ละ method แก้ Javadoc ของ class ให้ครอบคลุมทั้งสอง feature
ที่ใช้ได้เพิ่ม: `new MovieCatalog(movies).newestFirst(limit)`, `new MovieCatalog(movies).titlesSorted()`, `movies.clear()`,
`assertThat(shown).containsExactly(...)`

## 8 โจทย์ 8: pull request ที่อีกคน review

ชื่อ pull request: `Movie filter and sort scenarios` คำอธิบายใช้กล่องล่าง แทน `<issue>` ด้วยเลขจริง เก็บบรรทัดใต้ AI used for ที่ตรงกับคู่

<!-- block 8 pull request description -->
```markdown
## What
docs/intent and docs/ai: intent, assistant draft and our review
movie_filter.feature and movie_sort.feature: 4 scenarios (happy + sad)
with step definitions over MovieCatalog
Closes #<issue>
## How tested
./mvnw -B verify on B's machine: Tests run: 19, Failures: 0, Errors: 0
## AI used for
spec and story draft only (docs/ai/l3-draft.md), reviewed by both
(or: none, lab kit draft)
```

A review: อ่าน feature สองไฟล์และ MovieSteps ในแท็บ Files changed ถามอย่างน้อย 1 ข้อบนบรรทัดของ diff แล้ว Approve
ไม่มีใคร merge งานของตนเองโดยไม่มีคน review และห้าม merge ขณะ checks ยังแดง

## 9 งานนอกคาบของทีม

9.1 prompt สำหรับร่าง backlog ต่อท้ายด้วย `docs/intent/product.md` ของทีม (ไม่บังคับใช้ผู้ช่วย)

<!-- block 9.1 docs/ai/backlog-prompt.txt (first 10 lines) -->
```text
Read the intent.md below and draft a product backlog for it.

1. Write 8 to 12 user stories in the Connextra form (As a ..., So that ..., I want ...).
2. Give each story acceptance criteria: one happy path and at least one sad path.
3. Mark every story or criterion that the intent does not ask for.
4. List every assumption you made that is not in the intent, and every area of concern.

Do not write code. Keep to what the intent asks for.

--- intent.md ---
```

9.2 คำอธิบายของ Issue ของ story ทีมแต่ละเรื่อง (Title คือชื่อ feature)

<!-- block 9.2 team story Issue -->
```markdown
As a <บทบาทผู้ใช้>
So that <เป้าหมาย>
I want to <สิ่งที่ต้องทำ>

## Sad paths
- <เมื่อผิดพลาดแล้วระบบทำอะไร>

Drafted by: <ชื่อ หรือ assistant>
Confirmed by: <ใคร วันที่>
```

9.3 ข้อกำหนดของ story ทีม
| ข้อกำหนด | ตรวจอย่างไร |
|---|---|
| จำนวน | อย่างน้อย 6 เรื่องใน Backlog หรือ Icebox ของ board รวมของทุกคู่ในทีมไม่ซ้ำกัน |
| รูปแบบ | ทั้งสามบรรทัด Connextra ครบ และผ่าน SMART ทั้งห้าตัวอักษร |
| ขอบเขตของโจทย์ | รวมกันแล้วเห็น entity อย่างน้อย 3 ชนิด บทบาทผู้ใช้อย่างน้อย 2 แบบ และการเชื่อมต่อภายนอกอย่างน้อย 1 จุด |
| sad path | ทุกเรื่องมีหัวข้อ Sad paths อย่างน้อย 1 ข้อในเนื้อหา Issue |
| ประมาณการ | label points:1, points:2, points:3 หรือ points:5 ทุกเรื่อง เรื่องที่มีขนาดตั้งแต่ 5 point ขึ้นไปจะถูกแยกใน requirements clinic สัปดาห์ที่ 5 |
| ลำดับ | Backlog เรียงตามคุณค่าต่อลูกค้า ไม่ใช่ตามความง่าย |
| ที่มา | intent ของทีม commit ก่อนร่าง ร่างและการตรวจอยู่ใน docs/ai/ และ customer proxy ยืนยันทุกเรื่องก่อนเข้า Backlog |

แต่ละคู่เขียนหรือตรวจ 2 ถึง 3 เรื่อง แล้วอ่านของกันและกันก่อนใส่ board Iteration It-0 คือสัปดาห์ที่ 3 ถึง 5
story 6 เรื่องนี้คือจุดเริ่มของ Group assignment 1 ซึ่งต้องมี 12 เรื่องก่อนนำเสนอสัปดาห์ที่ 5

## 10 งานนอกคาบของคู่

| ไฟล์ใน docs/lofi/ | ภาพ |
|---|---|
| rp-stories.jpg | บัตร 3 ใบที่ตรวจแล้วจากโจทย์ 3 |
| rp-filter.jpg | Lo-Fi 3 หน้าจอจากโจทย์ 4 |
| story-<เลข Issue>-<slug>.jpg | storyboard ของ story ทีม 2 ไฟล์ เรื่องละ 3 หน้าจอขึ้นไป มีลูกศรและหน้าจอ sad path เช่น story-7-borrow-equipment.jpg |

ย่อภาพแต่ละไฟล์ให้ไม่เกินประมาณ 2 MB แล้วสร้าง `docs/L3.md` จากกล่องล่าง แก้ทุกส่วนในวงเล็บมุม ส่งผ่าน pull request ชื่อ
`L3 photos and passport` จาก branch `docs/l3` (คำสั่งใน README_kit.md) ให้ A review และ B merge

<!-- block 10 docs/L3.md -->
```markdown
# L3 passport
คู่: <GitHub username ของ A> และ <GitHub username ของ B> (ชื่อและรหัสส่งใน Moodle เท่านั้น)
repository: https://github.com/<A>/rottenpotatoes-pair-<A>-<B>
pull request story/<issue>-movie-filter: <ลิงก์> (review โดย A)
team board: <ลิงก์ GitHub Projects ของทีม>

## AI used for
- tool: <ชื่อและรุ่นของผู้ช่วย หรือ none: ใช้ร่างของ lab kit>
- prompt: docs/ai/l3-prompt.txt  ร่าง: docs/ai/l3-draft.md
- การตรวจ: docs/ai/l3-review.md  แก้ <n> ตัด <n> เก็บ <n>
- ตรวจอย่างไร: <เทียบ intent และ SMART, customer proxy ยืนยันเมื่อใด>
- ทีม: <ผู้ช่วยร่าง backlog หรือไม่ ไฟล์ใน docs/ai/ ของทีม>
- โค้ด, step definitions และ test: ไม่ได้ให้ผู้ช่วยเขียน
## สิ่งที่ส่ง
- intent: docs/intent/movie-filter.md (commit ก่อนร่าง)
- บัตร 3 ใบจากร่างที่ตรวจแล้ว: docs/lofi/rp-stories.jpg
- Lo-Fi ของโจทย์ 4: docs/lofi/rp-filter.jpg
- story ของทีมที่คู่เขียนหรือตรวจ: <ลิงก์ Issue>
- storyboard 2 เรื่อง: docs/lofi/story-...jpg
- movie_filter.feature และ movie_sort.feature เขียวใน GitHub Actions

## reflection
1. <sad path ของคู่จับความต้องการข้อใดที่ story บนบัตรไม่ได้พูดถึง และลูกค้าจะเห็นด้วยหรือไม่>
2. <step ที่ undefined แล้วเปลี่ยนถ้อยคำให้ตรงกับ step เดิมแทนการเขียน step ใหม่ เพราะเหตุใด>
3. <ร่างของผู้ช่วยผิดตรงไหนที่จะไม่เห็นถ้าไม่ได้เขียน intent ก่อน และ customer proxy แก้อะไรที่ผู้ช่วยรู้เองไม่ได้>
```

## 11 การส่งงานและเกณฑ์ lab passport

merge pull request ทั้งสองรายการแล้ว main ต้องมี Tests run: 19 และ badge เขียว `docs/L3.md` ครบ ลิงก์ board และ Issue เปิดได้
ภาพใน `docs/lofi/` ครบ 4 ไฟล์ A ส่งลิงก์ของ repository ใน Moodle ครั้งเดียวต่อคู่ ก่อนคาบสัปดาห์ที่ 4 ทั้งสองคนได้คะแนนเท่ากัน
คู่ที่ใช้ร่างของ kit ได้คะแนนเท่ากับคู่ที่ใช้ผู้ช่วยเอง ผู้สอนสุ่มตรวจประวัติ commit ประมาณ 20%

| คะแนน | เกณฑ์ |
|---|---|
| 0 | ไม่ส่ง |
| 1 | ส่งแล้ว แต่ GitHub Actions แดง หรือไม่มี pull request ที่ถูก review หรือไม่มี docs/L3.md |
| 2 | GitHub Actions เขียว scenario 4 ข้อ pull request มี review, intent ก่อนร่าง, l3-review.md 3 แถวขึ้นไป, story ทีม 6 เรื่องพร้อม points, AI used for ครบ |
| 3 | ระดับ 2 พร้อม storyboard 2 เรื่อง ภาพบัตร 3 ใบ และ reflection ครบ |

## 12 โจทย์เสริม (ไม่บังคับ): UML sketch ของ feature นี้

วาดบนกระดาษจากโค้ดของ L2: use case diagram ของ feature กรองและเรียงหนัง (actor และ use case) และ class diagram ของ class ที่
MovieSteps ใช้ แบบทั้ง class (attribute และ method ทั้งหมด ความสัมพันธ์ระหว่าง class) เฉลยคือ use case diagram ของ Rotten Potatoes
และ class diagram ของ class จาก L2 ในบรรยายสัปดาห์นี้ ที่ผู้สอนแสดงในคาบ
