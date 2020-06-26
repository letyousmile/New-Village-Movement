# JavaScript

##### Async / Await

- 비동기 코드를 작성하는 방법

- Async / await도 사실 promise 기반이다

- 절차적 언어럼 간단하고 이해하기도 쉬움

-  function 앞에 async 붙여주면 되고

- 내부의 promise를 반환하는 비동기 처리 함수 앞에 await 붙여주면 됨

- ```javascript
  function makeRequest() {
      return getData()
          .then(data => {
              if(data && data.needMoreRequest) {
                  return makeMoreRequest(data)
                    .then(moreData => {
                        console.log(moreData);
                        return moreData;
                    }).catch((error) => {
                        console.log('Error while makeMoreRequest', error);
                    });
              } else {
                  console.log(data);
                  return data;
              }
          }).catch((error) => {
            console.log('Error while getData', error);
          });
  }
  ```

- ```javascript
  async function makeRequest() { 
      try {
        const data = await getData();
        if(data && data.needMoreRequest) {
            const moreData = await makeMoreRequest(data);
            console.log(moreData);
            return moreData;
        } else {
            console.log(data);
            return data;
        }
      } catch (error) {
          console.log('Error while getData', error);
      }
  }
  ```

- await 의 장애물 - 한번에 한개만 기다릴 수 있음

