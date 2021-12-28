# SDPD-Final-Exam-Rubric

Final Exam Rubric
# CS F314 Final Exam: Questions
## Question 1 (10 pts)
Is it a good idea to update a UI element from a background task? Why or why not?
### Rubric
- mentions that it’s not a good idea = 3 pts
- discusses UI thread and main thread = 4 pts
- mentions any one of these reasons = 3 pts
  - ANR since a background task can potentially take an arbitrary amount of time
  - UI elements / activity can be destroyed while the background task is in progress, that may lead to null pointers
  - any other reasonable explanation should be considered too
## Question 2 (10 pts)
Suppose you are interacting with an app whose activity `SomeActivity` is on the screen. If you press the `Home` button, which lifecyle methods of `SomeActivity` will be called? Make sure you mention them in the proper order, and explain why any other activity lifecycle methods will not be called.
### Rubric
- stating `onPause` and `onStop` will be called in that order = 5 pts, only 2 pts if these methods mentioned but not in that order
- mentioning any other lifecycle method = -2 pts overall; may also mention `onSaveInstanceState` - no penalty for that
- justification:
  - `onDestroy`: must state in some way that the activity instance is not destroyed or that it is in the background = 3 pts
  - `onCreate`, `onStart`, `onResume` - 2 pts overall. Justification can be a single line based on the fact that the activity was not destroyed, so it will not be created, or a more detailed explanation.
## Question 3 (12 pts)
Consider this XML layout for an activity.
```xml
<?xml version=“1.0” encoding=“utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout ...>
  <Button
      android:id=“@+id/btn_add_book”
      android:layout_width=“wrap_content”
      android:layout_height=“wrap_content”
      android:text=“Add book”
      app:layout_constraintEnd_toEndOf=“parent”
      app:layout_constraintStart_toStartOf=“parent”
      app:layout_constraintTop_toBottomOf=“@+id/edit_author”
      android:contentDescription=“Click to add book” />
  <EditText
      android:id=“@+id/edit_title”
      android:layout_width=“wrap_content”
      android:layout_height=“wrap_content”
      app:layout_constraintEnd_toEndOf=“parent”
      app:layout_constraintStart_toStartOf=“parent”
      app:layout_constraintTop_toTopOf=“parent” />
  <EditText
      android:id=“@+id/edit_author”
      android:layout_width=“wrap_content”
      android:layout_height=“wrap_content”
      app:layout_constraintEnd_toEndOf=“parent”
      app:layout_constraintStart_toStartOf=“parent”
      app:layout_constraintTop_toBottomOf=“@+id/edit_title” />
</androidx.constraintlayout.widget.ConstraintLayout>
```
### Question 3.a (8 pts)
Suppose you run this app through Talk Back. Name two possible accessibility issues it will highlight.
### Rubric
Any two of these are ok = 4 pts each
- Content description says “Click to add book”. Talk back itself says “Click” or “Tap” for interactive buttons. So it will say “Click Click to add book” which might confuse a blind user.
- Neither of the EditText widgets have a hint text or content description. Talk back cannot give a meaningful information about these to the user.
### Question 3.b (4 pts)
Name one accessibility issue Talk Back will not highlight.
### Rubric
Any one of these is ok = 4 pts
- interactive widget size should be 48x48 dp - applicable for edit texts or the button
- contrast ratio for the text on the button or the edit texts may be insufficient
## Question 4 (8 pts)
Which of the following use cases do Fragments best support? Specify your choice and justify it briefly.
a) Rendering UI components flexibly for multiple screen sizes and orientations,
b) Making apps run slower,
c) Creating apps with fixed UI orientations,
d) Creating apps for smartphones only
### Rubric
- specifies option (a) = 3 pts
- justification using an example = 5 pt
  - most likely, they will mention a master/detail layout like in the slides
  - full points if reasonable explanation
  - dock 2-3 points if it’s not clear
## Question 5 (32 pts)
Consider the following Java code for an Android app.
```java
public class Star {
  private String mStarName;
  public Star(String starName) { mStarName = starName; }
  public String getStarName() { return mStarName; }
}
public class StarActivity extends AppCompatActivity {
  private List<Star> mStarList = new ArrayList<>();
  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_star);
    mStarList.add(new Star(“Proxima Centauri”));
    mStarList.add(new Star(“Our Sun”));
    mStarList.add(new Star(“Tau Ceti”));
    RecyclerView starRecyclerView = findViewById(R.id.recycler_view);
    StarAdapter starAdapter = new StarAdapter(mStarList);
    starRecyclerView.setAdapter(starAdapter);
  }
  private class StarHolder extends RecyclerView.ViewHolder {
    private String mStarStr;
    private TextView mStarTextView;
    StarHolder(LayoutInflater inflater, ViewGroup parent) {
      super(inflater.inflate(R.layout.list_item_star, parent, false));
    }
  }
  private class StarAdapter extends RecyclerView.Adapter<StarHolder> {
    private List<Star> mStarList;
    StarAdapter(List<Star> starList) { mStarList = starList; }
    @NonNull @Override
    public StarHolder onCreateViewHolder(@NonNull ViewGroup parent, int i) {
      LayoutInflater inflater = LayoutInflater.from(StarActivity.this);
      return new StarHolder(inflater, parent);
    }
    @Override
    public void onBindViewHolder(@NonNull StarHolder holder, int i) {
      Star star = mStarList.get(i);
    }
    @Override
    public int getItemCount() { return mStarList.size(); }
  }
}
```
### Question 5.a (8 pts)
Assuming `StarActivity` is the only activity in this app, explain in your own words what this app is doing.
### Rubric
- brief explanation of the list of stars and the recycler view = 4 pts
- the app does not really show the list = 4 pts
  - justification: `onBindViewHolder` is supposed to connect the view (e.g., a `TextView`) to the data (a star’s name in this case), but it doesn’t do that!
### Question 5.b (8 pts)
What do these lines of code do?
```java
StarAdapter starAdapter = new StarAdapter(mStarList);
starRecyclerView.setAdapter(starAdapter);
```
### Rubric
Explanation may vary but these points are expected
- a line or two about the adapter, what it’s supposed to do = 4 pts
- setting the adapter in the second line connects the recycler view with the adapter that contains the list of stars = 4 pts
### Question 5.c (8 pts)
What does these lines of code do? (10 points)
```java
LayoutInflater inflater = LayoutInflater.from(StarActivity.this);
return new StarHolder(inflater, parent);
```
### Rubric
Explanation may vary but these points are expected
- inflates the XML layout for the given context = 4 pts
- a new view holder is created that can render the items on the UI = 4 pt
### Question 5.d (8 pts)
Assume there is a floating action button named `button_fab` on the activity layout. If a user clicks this button, it should launch a new activity `AddStarActivity`. Assuming this activity class and layout is already present, write the piece of code that launches this activity on the button click. Clearly state any other assumptions you make. Clearly specify which method you are modifying and how.
### Rubric
Details may vary, judge accordingly. Only the launch of the new activity using an explicit is expected; returning the value is not required, but should not be penalised.
A simplest solution can be something like this:
```java
findViewById(R.id.button_fab).addOnClickListener(v -> {
    Intent intent = new Intent(this, AddStarActivity.class);
    startActivity(intent);
});
```
## Question 6 (10 pts)
Let’s consider the constraints you can put on a work manager request. Write the piece of code that creates a request that will run only if the device is connected to Wi-Fi. State any assumptions you make.
### Rubric
Again, actual code may vary, but it must have
- Creating a constraint with `NetworkType.UNMETERED` = 3 pts
- Creating a work request and calling `setConstraints` properly on it = 3 pts
- Other syntax and semantics = 2 pts
- Proper assumptions = 2 pts
  - must state that the a class extending `Worker` is properly defined
  - any other reasonable assumptions are fine
- Code to enque the request is not needed but shouldn’t be penalised
```java
Constraints constraints = new Constraints.Builder()
    .setRequriedNetworkType(NetworkType.UNMETERED)
    .build();
WorkRequest myWorkRequest = new OneTimeWorkRequest.Builder(MyWork.class)
    .setConstraints(constraints)
    .build();
```
## Question 7 (10 pts)
State whether you agree or disagree with each of the following statements? Justify your answer with an example in each case.
### Question 7.a (5 pts)
> You should avoid basing your alarm on clock time and use `ELAPSED_REALTIME` for repeating alarms whenever possible.
### Rubric
- states “agree” = 2 pts
- justification may include
  - scaling is a problem; if the alarm request is supposed to do something on a server, thousands of request may be fired at the same time
  - clock time may be affected by time zone changes
### Question 7.b (5 pts)
> You should not implement long-running operations in the `onReceive` methods of a Broadcast Receiver
### Rubric
- states “agree” = 2 pts
- justification may include
  - ANR, onReceive has a timeout of 10s
## Question 8 (8 pts)
What are some pros and cons of cross-platform development? Discuss at least two each.
### Rubric
Any two of these pros = 4 pts each
- reusable, shared codebase -> easy to maintain
- cost efficient since only one developere team, instead of separate teams for different OSs
- can also work for future/web platforms
Any two of these cons = 4 pts each
- may lose some platform-specific features/capabilities
- the end user does not get the “native” feel

