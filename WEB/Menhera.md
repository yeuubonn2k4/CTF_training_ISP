## Challenge

Chick Chick rất thích xem Menhera, trong một lần đang xem Mehera thì bỗng nhiên bị mất mạng.

`
) => {

    let speech = 'Konnichiawa! Bạn đang tìm mình để lấy flag đúng hem?? |Mình có điều kiện nho nhỏ|.......|Bạn phải ngắt kết nối mạng sau đó kết nối mạng lại 10 lần mình sẽ đưa flag cho bạn <3|Đếm nè: 10'
    
    function wordRunner(title) {
       
        document.querySelector('.title span').innerHTML = ''
        
        let i = 0
        
        function execRun() {
            
            if (i < title.length) {
               
                let char = title.charAt(i)
                
                if (char === '|') {
                  
                    document.querySelector('.title span').innerHTML = ''
                   
                    char = ''
               
                }
              
                document.querySelector('.title span').innerHTML += char
              
                ++i
                
                setTimeout(execRun, 50)
            
            }
        
        }
        
        execRun()
   
    }
    
    wordRunner(speech);
   
    let title = document.querySelector('.title span').innerHTML;
    
    let $$ = 10
    
    let $$$ = ['==QfzMzM881XxETMl9', '1XtNzXxQ2Xfhmbh91XhV3Yf9VawQ', '2XflGM291XzY3eiVHbjB3cpBiO5B', '6wuBSZn5WZsxWYoNGIhd6uhPGI', 'nFGbmByZu95uhDrxoRHIudqu', 'hjGcgA6wsBSeiOMkEDiLsOsc0', 'BibqOcarBSadub4wa8ZuBCoDzGI01quhjGdg4Wo6GuQ']
   
    let $$$$ = true
   
    const onlineEvent = () => {
       
        if (!$$$$) --$$
        
        document.querySelector('.title span').innerHTML = `Đếm nè: ${$$}`
        
        if ($$ === 0) {
          
            function $$$$$() {
            
                let flag = _.__($$$.join('').split('').reverse().join(''))
              
                wordRunner(flag)
           
            }
            
            return $$$$$();
       
        }
   
    }
  
    const offlineEvent = () => $$$$ = false
   
    $.addEventListener('online', onlineEvent)
  
    $.addEventListener('offline', offlineEvent)

})(window)

## Solution

- Doc code ta thay day la base64 bi dao nguoc va tach ra :v Ghep lai va decode no nao

`
import base64

a = ['==QfzMzM881XxETMl9', '1XtNzXxQ2Xfhmbh91XhV3Yf9VawQ', '2XflGM291XzY3eiVHbjB3cpBiO5B', '6wuBSZn5WZsxWYoNGIhd6uhPGI', 'nFGbmByZu95uhDrxoRHIudqu', 'hjGcgA6wsBSeiOMkEDiLsOsc0', 'BibqOcarBSadub4wa8ZuBCoDzGI01quhjGdg4Wo6GuQ']

b = ""

for i in a:
   
    b += i

b = base64.b64decode(b[::-1]).decode('utf-8')

print (b)
`

- Flag :

`
ispclub{v3__v0i__d0i__cua__anh__d1_3m__e111__<333}
`
