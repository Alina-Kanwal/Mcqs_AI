# Mcqs_AI 
Context drift ka matlab hai ke lambi conversation chalti rahe to model (ya agent) pichli baaton ka flow bhoolne lagta hai ya unko ghalat samajhne lagta hai. Isko avoid karne ke liye context summarization ya important info ko dubara inject karna use hota hai.
text ko readable aur styled banane ka tareeqa (documentation ya messages me).
Markdown ek lightweight markup language hai jo text ko easily format karne ke liye use hoti hai. Isme aap symbols aur characters (jaise #, *, -, ```) use karke headings, bold text, italic text, lists, tables aur code blocks bana sakte ho.
Agar tumne strict instructions de diye → wo apne domain ke bahar ke sawalon ko reject / refuse karega. warna ans dayga chahye irrelevent ho.
ModelSettings.resolve(). base, mid, top par mushtmil hota hy . mid base ki instruction ko or top mid ki instructions ko override krskta hy .
validate_json function ka kaam hota hai model ke JSON output ko check karna.....Agar _is_wrapped = True hai, iska matlab hai ke JSON hamesha is form me expected hai: { "response": ... } .....Agar "response" key miss ho jaye jab uski zaroorat thi, to SDK error throw karta hai (ModelBehaviorError) aur trace ke liye SpanError bhi attach karta hai.
max_turns ek safety cap hai jo agent ko infinite loop ya zyada tool calls karne se bachata hai.
Agar model 2nd turn pe bhi final answer nahi deta (sirf aur question poochta rehta), to MaxTurnsExceeded error raise hoti.
By default SDK agents me max_turns = 5 hota hai.
AgentOutputSchemaBase → skeleton / base.
AgentOutputSchema → asli output jo agent deta hai
RunHooks = basically RunHooks ka kaam hai: monitor, log, ya react krna run k har major event pe. ye wo hooks hain jo poory run (execution) k level pr lgty hain — jab tu Runner.run(...) chalati hai. inko use krky tu run ki lifecycle k events pakar skti hai.
RunResult =ye ek object hai jo Runner.run(...) k end me return hota hai. isme final outcome hota hai run ka.
normally isme hota hai:
output → jo final LLM / agent ne jawab dia
status → success, error, ya terminated
steps / turns info → agar detailed tracking on ho
kabhi kabhi metadata (jaise timings, tokens etc.)
RunHooks = “listener functions” jo run k dauran chalty hain.
RunResult = “final report” jo run complete hone k baad milti hai.



