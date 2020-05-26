## RecyclerView

> 기존 ListView에서는 종스크롤만 가능했기에 종, 횡 스크롤이 모두 가능한 ListView를 구현이 필요했다. ViewHolder는 View를 재사용할 수 있기에 메모리활용에도 장점(inflate와 findViewById() 호출이 줄어듬)이 있다. 기본 ListView도 ViewHolder를 적용한다면 View의 재사용이 가능하다. 하지만 커스텀 작업이 불편하기에 강제로 ViewHolder를 적용해야하는 RecyclerView를 사용한다. 또한 LayoutManager를 통해 스크롤의 방향과 형태를 자유자재로 바꿀수 있다는 장점과 데이터를 부분 수정 및 추가 삭제가 가능하다는 장점있다.
>
> ```java
> class RecyclerViewHolder extends RecyclerView.ViewHolder{
>  
>  @BindView(R.id.text_view)
>  TextView textView;
>  @BindeView(R.id.layout)
>  LinearLayout linearLayout;
> 
>  RecyclerViewItemClickListener recyclerViewItemClickListener;
>  
>  public RecyclerViewHolder(@NonNull View itemView){
>      super(itemView);
>      ButterKnife.bind(this, itemView);
>  }
>  
>  public void setItem(Item item){
>      textView.setText(item.name);
>  }
>  //Click Listener 연결해줌
>  public void onBind(int position){
>      linearLayout.setOnClickListener(new View.onClickListener(){
>          @Override
>          public void onClick(View view){
>              recyclerViewItemClickListener.onItemClick(position);
>          }
>      });
>  }
>  
>  public void setOnItemClickListener
>      (RecyclerViewItemClickListener recyclerViewItemClickListener){
>      this.recyclerViewItemClickListener = recyclerViewItemClickListener;
>  }
>  
> }
> ```
>
> 기본적인 RecyclerViewHolder 구현,  ClickListener를  연결해주고, Adapter를 통해 넘어온 데이터를 View에 뿌려주는 형태.
>
> ```java
> class RecyclerViewAdapter extends RecyclerView.Adpater<RecyclerViewHolder>{
>  
>  ArrayList<Item> arrayList = new ArrayList<>();
>  RecyclerViewItemClickListener recyclerViewItemClickListener;
>      
>  @NonNull
>  @Override
>  public RecyclerViewHolder onCreateViewHolder(@NonNull ViewGroup viewGroup,
>                                               int viewType){
>      return new RecyclerViewHolder(LayoutInflater
>                                    .from(viewGroup.getContext)
>                                    .inflate(R.layout.recycler_item, 
>                                          viewGroup, 
>                                          flase));
>  }
>  
>  @Override
>  public void onBindViewHolder(@NonNull RecyclerViewHolder recyclerViewHolder,
>                               int position) {
>      //ViewHolder의 ClickListener 등록해줌
>      recyclerViewHolder.setOnItemClickListener(itemClickListenr);
>      //클릭시 아이템 개별 리스너 등록
>      recyclerViewHolder.onBind(position);
>      //ViewHolder의 View들 값을 연결해줌
>      recyclerViewHolder.setItem(getItem(position));
>  }
>  
>  @Override
>  public int getItemCount(){
>      return arrayList.size();
>  }
>  
>  public Item getItem(int position){
>      if(arrayList.size()!= 0)
>      	return arrayList.get(position);
>      else
>          return new Item("Error");
>  }
>  public void setOnItemClickListener
>      (RecyclerViewItemClickListener recyclerViewItemClickListener){
>      this.recyclerViewItemClickListener = recyclerViewItemClickListener;
>  }
> }
> ```
>
> 기본적인 RecyclerViewAdapter 구현, onCreateViewHolder , onBindViewHolder, getItemCount 이 3가지의 함수는 필수적으로 구현해야 하는 함수이다.
>
> **getItemCount**는 Adapter가 관리하는 아이템의 갯수를 반환한다. 즉 아이템의 갯수 만큼만 ViewHolder를 생성한다. 
>
> **onCreateViewHolder**에서 각 아이템의 View를 바인딩하고,  바인딩한 View를 ViewHolder에 넘기면서 ViewHolder를 생성한다.
>
> **onBindViewHolder**에서 Listener를 등록해주고, position 변수를 통해 해당 되는 position의 아이템을 View에 Set 해준다.
>
> **getItemCount**를 통해 ViewHolder의 갯수를 정하고 **onCreateViewHolder -> onBindViewHolder**를 갯수만큼 실행한다.
>
> ```java
> class MainActivity extends AppCompatActivity{
>  @BindView(R.id.recycler_view)
>  RecyclerView recyclerView;
>  
>  @Override
>  protected void onCreate(Bundle savedInstanceState){
>      super.onCreate(savedInstanceState);
>      setContView(R.layout.activity_main);
>      
>      ButterKnife.bind(this);
>      
>      //스크롤의 방향이나 모양 즉 리스트뷰안의 구성을 결정하는 layoutmanager
>      LinearLayoutManager layoutManager = 
>          new LinearLayoutManager(context,LinearLayoutManager.VERTICAL, false);
>    
>      RecyclerViewAdapter recyclerViewAdapter = new RecyclerViewAdapter();
>      
>      recyclerViewAdapter.setOnItemClickListener
>          (new RecyclerViewItemClickListener(){
>              @Override
>              public void onItemClick(int position) {
>                  //Todo..
>              }
>      });
>      recyclerView.setLayoutManager(layoutManager);//layoutmanager를 연결해줌
>      recyclerView.setAdapter(recyclerViewAdapter);//adapter를 연결해줌
>      
>  }
> }
> ```
>
> Activity에서 실제로 사용하는 코드, RecyclerView를 생성 및 바인딩을 해준다. 자신이 사용하고자 하는 LayoutManager를 생성후 RecyclerView에 Set 해준다.  RecyclerViewAdapter를 생성후 마찬가지로 RecyclerView에 Set 해줌으로서 RecyclerView를 사용할수 있다. 추가적으로 addItem 등을 구현하여 RecyclerView 안의 아이템을 채워주면 된다. 
>
> 보다 자세한 내용은 [여기](https://medium.com/@bansooknam/android-recyclerview-%EC%9A%94%EC%95%BD-aaea4a9c95e7)를 참조하면 된다.
> 
> 2018년 2월에 **ListAdapter**가 추가되었다. 더 공부해 보면 좋을 것이다.