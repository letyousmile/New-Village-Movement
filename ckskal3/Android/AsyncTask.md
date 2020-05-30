## AsyncTask

> Android는 보통 UI를 조작하는 Main Thread를 두고 작업을 진행하는데 이 Thread에서 시간이 오래걸리는 서버와의 통신등의 작업을 진행하면 UI를 변경하거나 하는 중요한 작업들을 못하는 경우가 생기므로 따로 Thread를 두고 통신과 같은 시간이 오래 걸리는 작업을 진행해야한다.  이런경우에 사용하기 위해 AsyncTask라는 클래스를 만들었다. 
>
> ### 클래스 구성
>
> - onPreExcuted
>   - Main Thread에서 실행되며 준비작업을 하는 부분, 보통 프로그래스바를 띄워서 백그라운드에서 작업중임을 알려준다.
> - doInBackground
>   - 실질적으로 백그라운드 작업을 하는 부분, 보통 서버와의 통신을 통해 필요한 데이터를 받아온다.
> - onPostExcuted
>   - 백그라운드 작업이 끝나고 받아온 데이터를 바탕으로 UI에 매치하는 등의 작업을 한다.
> - onProgressUpdte / publishProgress
>   - doInBackground에서 작업을 하는 중 UI를 바꾸고 싶거나 중간중간의 데이터를 보내고 싶을때 사용하는 부분,  publishProgress 함수를 호출하면 MainThread에서 onProgressUpdate를 통해 UI를 변경하는 등의 작업을 한다.
>
> ```java
> class AsyncTaskExample extends AsyncTask<String, Integer , String>{
> 									 //Params, Progress, Result 순으로 Generic 설정
> @Override
> protected void onPreExecute() {
>   super.onPreExecute();
>   //Todo...
> }
> 
> @Override
> protected String doInBackground(String... params) {
>   //Todo...
>   publishProgress(10);
>   return "Result";
> }
> 
> @Override
> protected void onPostExecute(String result) {
>   super.onPostExecute(result);
>   //Todo...
> }
> 
> @Override
> protected void onProgressUpdate(Integer... values) {
>   super.onProgressUpdate(values);
>   //Todo...
> }
> }
> 
> @OnClick(value = R.id.view1)
> public void onClickTest(){
> new AsyncTaskExample().execute();
> }
> ```
>
> AsyncTask로 구현한 객체는 하나의 객체이므로 재사용이 불가능하다. 객체를 새로 생성은 가능하지만 메모리 효율이 떨어지기에 하지 않는것을 권함. 
>
> 처음에는 싱글스레드에서 순차처리였다가 ''도넛''이 나온후 병렬처리로 바뀌었다. 다시 ''허니콤''이 나온 이후 병렬 처리에대한 이슈로 인해 다시 싱글 스레드를 기본으로 바뀌었다. 이때 병렬 처리를 원한다면 executeOnExecutor(  AsyncTask.THREAD_POOL_EXECUTOR) 을 사용하면 된다.
