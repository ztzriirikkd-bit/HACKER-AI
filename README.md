const API_KEY = "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";

const chatBox = document.getElementById("chatBox");

/* resize */
function resize(el){

  el.style.height = "auto";

  el.style.height = el.scrollHeight + "px";

}

/* suggestions */
function useSugg(btn){

  document.getElementById("inp").value =
  btn.innerText;

  send();

}

/* enter */
function handleKey(e){

  if(e.key === "Enter" && !e.shiftKey){

    e.preventDefault();

    send();

  }

}

/* add message */
function addMsg(text,type){

  const empty =
  document.getElementById("emptyState");

  if(empty) empty.remove();

  let div = document.createElement("div");

  div.className = "msg " + type;

  div.innerText = text;

  chatBox.appendChild(div);

  chatBox.scrollTop = chatBox.scrollHeight;

  return div;

}

/* AI send */
async function send(){

  let input =
  document.getElementById("inp");

  let text = input.value.trim();

  if(!text) return;

  addMsg(text,"user");

  input.value = "";

  input.style.height = "auto";

  let loading =
  document.createElement("div");

  loading.className = "msg bot";

  loading.innerHTML = `
  <div class="typing">
    <span></span>
    <span></span>
    <span></span>
  </div>
  `;

  chatBox.appendChild(loading);

  chatBox.scrollTop = chatBox.scrollHeight;

  try{

    let res = await fetch(
      "https://openrouter.ai/api/v1/chat/completions",
      {

        method:"POST",

        headers:{
          "Authorization":"Bearer " + API_KEY,
          "Content-Type":"application/json",
          "HTTP-Referer":"https://localhost",
          "X-Title":"HACKER TEAM AI"
        },
