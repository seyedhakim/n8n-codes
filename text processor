// دریافت متن خام
// Accessing input data
const rawText = items[0].json.raw_text;

const results = [];

// تقسیم متن بر اساس فصل‌ها
// Splitting by Chapters
const chapters = rawText.split(/(?=❯\s*فصل)/);

let currentChapter = "مقدمه یا بدون فصل";

for (const section of chapters) {
    // استخراج نام فصل
    if (section.trim().startsWith("❯")) {
        const lines = section.split('\n');
        currentChapter = lines[0].replace('❯', '').trim();
    }

    // *** تغییر مهم اینجاست ***
    // ما یک \n به ابتدای regex اضافه کردیم تا فقط ماده‌هایی که اول خط هستند را بگیرد
    // Splitting by Articles (Only if starts with new line)
    const articles = section.split(/(?=\nماده\s+\d+)/);

    for (let i = 0; i < articles.length; i++) {
        let content = articles[i].trim();

        // چک می‌کنیم بخش با "ماده" شروع شود
        if (content.startsWith("ماده")) {
            // پیدا کردن خط اول (شماره ماده) و بقیه متن
            const firstNewLineIndex = content.indexOf('\n');
            
            // اگر خط جدیدی نبود، کل محتوا شماره ماده است (جلوگیری از خطا)
            if (firstNewLineIndex === -1) continue; 

            const articleNum = content.substring(0, firstNewLineIndex).trim(); // مثلا: ماده 1
            const articleText = content.substring(firstNewLineIndex).trim();

            // مدیریت خاص برای ماده 52 و پایان فایل
            // Special handling for Article 52
            if (articleNum.includes("52")) {
                // جدا کردن بخش پایانی با عبارت مشخص شده
                const parts = articleText.split("قانون فوق مشتمل بر");
                
                // بخش اول: خود ماده 52
                results.push({
                    json: {
                        article_number: articleNum,
                        article_text: parts[0].trim(),
                        article_related_chapter: currentChapter
                    }
                });

                // بخش دوم: متن پایانی (اگر وجود داشته باشد)
                if (parts.length > 1) {
                    results.push({
                        json: {
                            article_number: "end",
                            article_text: "قانون فوق مشتمل بر" + parts[1].trim(), // متن را کامل برمی‌گردانیم
                            article_related_chapter: currentChapter
                        }
                    });
                }
            } else {
                // ماده‌های معمولی
                results.push({
                    json: {
                        article_number: articleNum,
                        article_text: articleText,
                        article_related_chapter: currentChapter
                    }
                });
            }
        }
    }
}

return results;
