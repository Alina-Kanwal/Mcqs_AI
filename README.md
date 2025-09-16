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
RunResult → final result at the end.
RunResultStreaming → result milta hai stream k form me, step-by-step / token-by-token.
FunctionTool se bhi wahi kaam hoga jo @function_tool decorator se hota tha, bs yahan tu manual control le rahi hai. FunctionTool my hum function ko tool bagair decorator lagaye banaty hain.
🔹FunctionTool
jab tu koi FunctionTool run krti hai, uska output wrap ho k aata hai FunctionToolRes..
isme hota hai:
tool ka naam
inputs jo diye gaye the
output jo tool ne return kia
🔹 ComputerTool
ye OpenAI ka ek hosted tool hai.
isse tu computer actions krwa skti hai jaisy:
file banana / read karna
screenshot lena
apps open karna
🔹 CodeInterpreterTool
ye bhi ek OpenAI hosted tool hai.
ye LLM ko ek Python execution environment deta hai.
jahan wo code likh k run kr skta hai:
data analysis
graph banana
calculations
file processing
FunctionToolResult → tool ke run hone ka nateeja (result container).
ComputerTool → LLM ko ek computer environment deta hai.
CodeInterpreterTool → LLM ko Python run krne ka environment deta hai.
🔹 ImageGenerationTool
iska kaam hai text se image generate krna (text-to-image), jab LLM ko kahoge “make an image of a cat on the moon” → ye tool use hoga aur image return karega.
🔹 LocalShellTool
LocalShellTool ka matlab hai ke tu LLM ko apne system ke terminal/shell ka access de rahi. mtlb ls, pwd, echo hello jesi commands run krwa skta hai.
🔹 LocalShellCommandRequest
ye ek request object hota hai jo LLM bhejta hai jab usay koi shell command chalani hoti hai.
ye request LocalShellTool ko pass hota hai
LocalShellTool apna LocalShellExecutor use karta hai
LocalShellExecutor actually system ke terminal/shell me command run karta hai
output result wapis LLM ko bhej diya jata hai.
LLM → LocalShellCommandRequest → LocalShellTool → LocalShellExecutor → System Shell → Output → back to LLM
🔹 HandoffInputData
ye ek data object hota hai jo handoff ke waqt user ka input aur agent ka context(agent ko di gai info) carry karta hai.
👉 matlab jab ek agent query doosre agent ko handoff karta hai → to ye HandoffInputData ke form me transfer hoti hai.
🔹 HandoffInputFilter
ye ek function ya filter hota hai jo decide karta hai ke:
"kya ye user input handoff hona chahiye ya nahi?"
HandoffInputData → jo input data handoff ke saath transfer hota hai.
HandoffInputFilter → ek filter function jo decide karta hai input handoff ke laayak hai ya nahi.
🔹 InputGuardrailResult
jab user ka input guardrail se guzarta hai, uska result isi object me wrap hota hai.
input allowed hai ya blocked, agar blocked hai to kya reason hai , aur agent ko kya fallback message dena chahiye.
🔹 OutputGuardrailResult
jab LLM apna jawab banata hai, wo guardrail check hota hai aur uska result isi object me hota hai.
output pass hua ya fail, agar fail hua to kya karna hai (block/modify/replace), aur agent ko kya final safe output bhejna hai.
🔹 OpenAIChatCompletionsModel
ye ek wrapper class hai jo OpenAI ke chat completions endpoint ko use karne ke liye hoti hai.
yani agar aapko "gpt-4.1" ya "gpt-4o-mini" se baat karni hai chat style me, to ye use hota hai.
🔹 OpenAIResponsesModel
ye OpenAI ke Responses API ko handle karta hai (jo new aur advanced API hai).
ye sirf chat nahi, balki text, structured outputs, tool calls sab ko support karta hai. Ye zada flexible hota hy.
🔹 OpenAIProvider
ye ModelProvider ki tarah hai lekin specifically OpenAI ke liye., ab agent ko run karte ho aur use kehte ho provider="openai", to wo OpenAIProvider use karega.
🔹 RunItem -> 
ab jab Runner chal raha hota hai, to wo different steps / items bana ke rakhta hai aik item RunItem.
🔹 RunItemStreamEvent
ab jab Runner chal raha hota hai, to wo different steps / items bana ke rakhta hai:
ek item ho sakta hai → LLM ka ek partial jawab
doosra item ho sakta hai → koi tool call jo LLM ne invoke kiya
teesra item → us tool ka result
jab bhi koi aisa naya item create hota hai ya update hota hai, us waqt ek RunItemStreamEvent trigger hota hai.
🔹 AgentUpdatedStreamEvent
agent ka plan change hua, koi naye tools enable huye, agent ne naya instruction bana liya.
🔹 TResponseInputItem → input side (jo agent ko diya jaata hai, user-prompt bhi isme aata hai).
🔹 MessageOutputItem → output side (jo agent user ko wapas bhejta hai).
🔹 ModelResponse
Ye represent karta hai LLM ka raw jawab Yani obj structure(OpenAI, Anthropic, ya jo bhi model use ho raha hai).
Isme tokens, reasoning trace, aur completion text waghera hota hai..
------------------------------------------------------------------------------Items and Data Structures
🔹 HandoffCallItem
Ye ek special type ka RunItem hai jo tab banta hai jab agent decide karta hai ke request kisi aur agent ko forward karni hai.
Isme info hoti hai kis agent/service ko handoff karna hai.
🔹 HandoffOutputItem ki role
HandoffOutputItem sirf ek martaba nahi, balki multiple handoffs ke liye bhi use hota hai.
Agar pehle agent ne request ko second agent ko forward kiya, aur
Second agent ne process karte waqt dekha ke actually ye kaam third agent ka hai,
👉 To second agent phir se ek naya HandoffOutputItem bhejega.
HandoffOutputItem → action (handoff perform karna).
HandoffCallItem → record (handoff action ka log maintain karna).
🔹 ToolCallItem
Ye ek RunItem type hai jo tab banta hai jab agent decide karta hai ke mujhe koi tool chalana hai.
Isme tool ka naam aur arguments (inputs) hote hain.
🔹 ToolCallOutputItem
Ye ek OutputItem hai jo tab banta hai jab tool ka result wapas aata hai.
Isme tool ka naam + uska output/result hota hai.
ToolCallItem → Request (Agent ne bola: ye tool chalao).
ToolCallOutputItem → Response (Tool ka jawab: ye result aya).
🔹 ReasoningItem
Matlab: jab model internally step-by-step reasoning likhta hai (jo normally user ko nahi dikhai jati), SDK usko ek ReasoningItem me capture kar leta hai.

















