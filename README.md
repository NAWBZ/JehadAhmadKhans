# JehadAhmadKhans
# ETEA27_practiceTest_online

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>ETEA Practice Test</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
<style>
body{font-family:'Poppins',sans-serif;background:linear-gradient(135deg,#ff9a9e,#fecfef);margin:0}
.container{max-width:900px;margin:20px auto;background:#fff;padding:30px;border-radius:20px;box-shadow:0 10px 30px rgba(0,0,0,.2)}
h2{text-align:center}
.center{text-align:center}
input{width:100%;padding:12px;margin-bottom:15px;border-radius:10px;border:2px solid #ddd}
button{padding:12px;border:none;border-radius:10px;font-size:16px;font-weight:bold;cursor:pointer;margin:5px}
#startBtn{background:#2980b9;color:#fff;width:100%}
#timer{background:#e74c3c;color:#fff;padding:15px;text-align:center;border-radius:10px;margin-bottom:10px}
.stats-bar{display:flex;justify-content:space-between;background:#f8f9fa;padding:10px;border-radius:10px;margin-bottom:15px;font-weight:600;font-size:14px;border:1px solid #eee}
.option{border:2px solid #ddd;padding:15px;border-radius:10px;margin-bottom:12px;cursor:pointer}
.option.correct{background:#2980b9;color:#fff}
.option.wrong{background:#e74c3c;color:#fff}
.buttons{display:flex;justify-content:space-between}
#submitBtn{display:none;background:#27ae60;color:white;width:100%;margin-top:20px}
.subject-box{background:#f4f6f7;padding:12px;border-radius:10px;margin:8px 0}
.review-option.correct{background:#2980b9;color:#fff;padding:8px;border-radius:8px;margin:4px 0}
.review-option.wrong{background:#e74c3c;color:#fff;padding:8px;border-radius:8px;margin:4px 0}
.footer{text-align:center;color:#555;margin:20px 0;font-size:14px}
#resName{font-size: 22px; color: #2980b9; font-weight: 600;}
</style>
</head>
<body>

<div class="container center" id="loginDiv">
<h2>ETEA Practice Test</h2>
<input id="studentName" placeholder="Student Name">
<input id="rollNo" placeholder="Roll Number">
<button id="startBtn" onclick="startCountdown()">Start Test</button>
<div id="countdown" style="font-size:28px;color:red"></div>
</div>

<div class="container" id="quizDiv" style="display:none">
<div id="timer">Time Left: 50:00</div>
<div class="stats-bar">
    <span id="qCounter">Question: 1/100</span>
    <span id="attemptedCounter">Attempted: 0/100</span>
</div>
<div id="questionBox"></div>
<div id="optionsBox"></div>
<div class="buttons">
<button onclick="prevQ()">Previous</button>
<button onclick="skipQ()">Skip</button>
<button onclick="nextQ()" id="nextBtn" disabled>Next</button>
</div>
<button id="submitBtn" onclick="submitTest()">Submit Test</button>
</div>

<div class="container center" id="resultDiv" style="display:none">
<h2 id="resStatus"></h2>
<p id="resName"></p>
<p id="resScore"></p>
<p id="resTime"></p>
<h3>Subject-Wise Result</h3>
<div id="subjectResult"></div>
<button onclick="showReview()">Review Test</button>
<button onclick="location.reload()">Restart</button>
</div>

<div class="container" id="reviewDiv" style="display:none">
<h2 class="center">Test Review</h2>
<div id="reviewContent"></div>
<button onclick="backToResult()">Back to Result</button>
</div>

<div class="footer">
Created by <b>JEHAD AHMAD KHAN FARAZ</b>
</div>

<script>

 
 const questions = [
    // Common Noun & Basic Noun Concepts (1-30)

    {subject:"English", q:"A noun is a word used to name:", o:["Action","Person, place or thing","Quality","Doing word"], c:1},

    {subject:"English", q:"Which of the following is NOT a noun?", o:["Beauty","Beautiful","Pakistan","Teacher"], c:1},

    {subject:"English", q:"Identify the noun: \"Honesty is the best policy\"", o:["Best","Is","Honesty","Policy"], c:2},

    {subject:"English", q:"Nouns can function as:", o:["Subject only","Object only","Subject and Object","Verb"], c:2},

    {subject:"English", q:"The word \"running\" in \"Running is good exercise\" is a:", o:["Verb","Adverb","Noun","Adjective"], c:2},

    {subject:"English", q:"Which word shows possession?", o:["Ali's book","Ali runs","Ali is good","Ali went"], c:0},

    {subject:"English", q:"Abstract nouns name things we:", o:["Can touch","Cannot touch","Can see","Can eat"], c:1},

    {subject:"English", q:"\"The crowd was cheering\" - 'crowd' is:", o:["Common noun","Collective noun","Proper noun","Abstract noun"], c:1},

    {subject:"English", q:"Nouns are often preceded by:", o:["Verbs","Articles a, an, the","Adverbs","Prepositions"], c:1},

    {subject:"English", q:"\"Peshawar\" is a noun because it names a:", o:["Person","Place","Thing","Idea"], c:1},

    {subject:"English", q:"Which is a concrete noun?", o:["Love","Chair","Anger","Happiness"], c:1},

    {subject:"English", q:"Gerund is a noun formed from:", o:["Adjective","Verb + ing","Adverb","Preposition"], c:1},

    {subject:"English", q:"\"Children\" is the plural of:", o:["Child","Children","Childs","Childes"], c:0},

    {subject:"English", q:"Proper nouns always begin with:", o:["Small letter","Capital letter","Number","Symbol"], c:1},

    {subject:"English", q:"\"Water\" can be:", o:["Only countable","Only uncountable","Both","None"], c:2},

    {subject:"English", q:"Identify the noun: \"The teacher teaches well\"", o:["Teaches","Well","The","Teacher"], c:3},

    {subject:"English", q:"Which suffix often makes abstract nouns?", o:["-ing","-ness","-ly","-er"], c:1},

    {subject:"English", q:"\"Rice\" is a:", o:["Countable noun","Uncountable noun","Proper noun","Collective noun"], c:1},

    {subject:"English", q:"Compound nouns are made of:", o:["One word","Two or more words","Verb + ing","Adjective"], c:1},

    {subject:"English", q:"\"Team\" is a collective noun because it shows:", o:["One person","Group of persons","Place","Thing"], c:1},

    {subject:"English", q:"Which is not a rule to identify noun?", o:["Comes after article","Can have 's","Always ends in -ing","Can be plural"], c:2},

    {subject:"English", q:"\"Freedom\" is an:", o:["Common noun","Proper noun","Abstract noun","Collective noun"], c:2},

    {subject:"English", q:"Nouns can be made plural by adding:", o:["-ly","-s or -es","-ing","-ed"], c:1},

    {subject:"English", q:"\"Ali and Ahmed\" - both are:", o:["Common nouns","Proper nouns","Abstract nouns","Collective nouns"], c:1},

    {subject:"English", q:"\"Bravery\" is formed from adjective:", o:["Brave","Bravely","Bravest","Braver"], c:0},

    {subject:"English", q:"Which comes before noun to show quantity?", o:["Run","Much","Quickly","Go"], c:1},

    {subject:"English", q:"\"Khyber Pakhtunkhwa\" is a:", o:["Common noun","Proper noun","Abstract noun","Material noun"], c:1},

    {subject:"English", q:"Nouns can be:", o:["Only singular","Only plural","Singular and Plural","Neither"], c:2},

    {subject:"English", q:"\"Gold\" is a:", o:["Common noun","Proper noun","Material noun","Abstract noun"], c:2},

    {subject:"English", q:"The best test to identify a noun is:", o:["It shows action","It can be replaced by pronoun","It ends in -ly","It shows time"], c:1},


    // Common & Proper Noun (31-33)

    {subject:"English", q:"\"City\" is a:", o:["Common noun","Proper noun","Abstract noun","Collective noun"], c:0},

    {subject:"English", q:"\"Islamabad\" is a:", o:["Common noun","Proper noun","Abstract noun","Material noun"], c:1},

    {subject:"English", q:"All names of persons are:", o:["Common nouns","Proper nouns","Abstract nouns","Collective nouns"], c:1},


    // Collective Noun (34-36)

    {subject:"English", q:"\"Army\" is a:", o:["Common noun","Proper noun","Collective noun","Abstract noun"], c:2},

    {subject:"English", q:"\"Class\" refers to:", o:["One student","Group of students","Teacher","Book"], c:1},

    {subject:"English", q:"\"Fleet\" is used for:", o:["Birds","Ships","Cattle","People"], c:1},


    // Abstract Noun (37-39)

    {subject:"English", q:"\"Kindness\" is an:", o:["Common noun","Abstract noun","Proper noun","Material noun"], c:1},

    {subject:"English", q:"Abstract nouns express:", o:["Physical things","Feelings and qualities","Places","Animals"], c:1},

    {subject:"English", q:"\"Childhood\" is formed from:", o:["Child","Childish","Childlike","Children"], c:0},


    // Material Noun (40-42)

    {subject:"English", q:"\"Wood\" is a:", o:["Common noun","Proper noun","Material noun","Abstract noun"], c:2},

    {subject:"English", q:"Material nouns are:", o:["Countable","Uncountable","Both","None"], c:1},

    {subject:"English", q:"\"Silver\" is used to make:", o:["Furniture","Jewelry","Clothes","Books"], c:1},


    // Countable & Uncountable Noun (43-47)

    {subject:"English", q:"\"Book\" is:", o:["Countable","Uncountable","Abstract","Material"], c:0},

    {subject:"English", q:"\"Sugar\" is:", o:["Countable","Uncountable","Proper","Collective"], c:1},

    {subject:"English", q:"We use \"many\" with:", o:["Uncountable","Countable","Abstract","Material"], c:1},

    {subject:"English", q:"\"Information\" is:", o:["Countable","Uncountable","Common","Proper"], c:1},

    {subject:"English", q:"\"Air\" is a:", o:["Countable noun","Uncountable noun","Collective noun","Proper noun"], c:1},


    // Concrete Noun (48-49)

    {subject:"English", q:"Concrete nouns can be perceived by:", o:["Mind","Five senses","Heart","Soul"], c:1},

    {subject:"English", q:"\"Table\" is a:", o:["Abstract noun","Concrete noun","Proper noun","Collective noun"], c:1},


    // Compound Noun (50-51)

    {subject:"English", q:"\"Classroom\" is a:", o:["Simple noun","Compound noun","Abstract noun","Material noun"], c:1},

    {subject:"English", q:"\"Toothbrush\" is made of:", o:["Noun + Noun","Verb + Noun","Adj + Noun","All"], c:0},
];
  
let answers=new Array(questions.length).fill(null);
let index=0,time=3000,startTime,timerInt;

function startCountdown(){
if(!studentName.value||!rollNo.value){alert("Fill all fields");return;}
let c=5;countdown.innerText=c;
let i=setInterval(()=>{
c--;countdown.innerText=c;
if(c===0){
clearInterval(i);
loginDiv.style.display="none";
quizDiv.style.display="block";
startTime=Date.now();
loadQ();startTimer();
}},1000);
}

function updateStats() {
    let attempted = answers.filter(a => a !== null).length;
    document.getElementById("qCounter").innerText = `Question: ${index + 1}/${questions.length}`;
    document.getElementById("attemptedCounter").innerText = `Attempted: ${attempted}/${questions.length}`;
}

function loadQ(){
let q=questions[index];
questionBox.innerHTML=`<h3>${index+1}. (${q.subject}) ${q.q}</h3>`;
optionsBox.innerHTML="";
q.o.forEach((op,i)=>{
let d=document.createElement("div");
d.className="option";
if(answers[index] === i) d.classList.add(i===q.c?"correct":"wrong");
d.innerText=op;
d.onclick=()=>{
if(answers[index]!=null)return;
answers[index]=i;
d.classList.add(i===q.c?"correct":"wrong");
nextBtn.disabled=false;
updateStats();
};
optionsBox.appendChild(d);
});
nextBtn.disabled=answers[index]==null;
submitBtn.style.display=index===questions.length-1?"block":"none";
updateStats();
}

function nextQ(){if(index<questions.length-1){index++;loadQ();}}
function prevQ(){if(index>0){index--;loadQ();}}
function skipQ(){index=(index+1)%questions.length;loadQ();}

function startTimer(){
timerInt=setInterval(()=>{
let m=Math.floor(time/60),s=time%60;
timer.innerText=`Time Left: ${m}:${s<10?'0':''}${s}`;
if(time--<=0){clearInterval(timerInt);submitTest();}
},1000);
}

function submitTest(){
clearInterval(timerInt);
quizDiv.style.display="none";
resultDiv.style.display="block";
let score=0,subjectStats={};
questions.forEach((q,i)=>{
if(!subjectStats[q.subject]) subjectStats[q.subject]={total:0,correct:0};
subjectStats[q.subject].total++;
if(answers[i]===q.c){score++;subjectStats[q.subject].correct++;}
});
let pct=Math.round(score/questions.length*100);
resStatus.innerText=pct>=40?"PASSED":"FAILED";
resName.innerText=`Student Name: ${studentName.value}`;
resScore.innerText=`Overall Score: ${score}/${questions.length} (${pct}%)`;
resTime.innerText=`Time Taken: ${Math.floor((Date.now()-startTime)/60000)} minutes`;
subjectResult.innerHTML="";
for(let s in subjectStats){
let x=subjectStats[s];
subjectResult.innerHTML+=`<div class="subject-box"><b>${s}</b>: ${x.correct}/${x.total} → ${Math.round(x.correct/x.total*100)}%</div>`;
}
}

function showReview(){
resultDiv.style.display="none";
reviewDiv.style.display="block";
reviewContent.innerHTML="";
questions.forEach((q,i)=>{
let html=`<h4>${i+1}. ${q.q}</h4>`;
q.o.forEach((op,idx)=>{
let cls="review-option";
if(idx===q.c) cls+=" correct";
else if(answers[i]===idx) cls+=" wrong";
html+=`<div class="${cls}">${op}</div>`;
});
reviewContent.innerHTML+=html;
});
}

function backToResult(){
reviewDiv.style.display="none";
resultDiv.style.display="block";
}
</script>
</body>
</html>
