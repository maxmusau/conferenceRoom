# conferenceRoom
Conference room Reservation Project
# Recycler View - In this Project we create a Recycler using data from an online API,
## RecyclerView is the ViewGroup that contains the views corresponding to your data. · Each individual element in the list is defined by a view holder object
#In the Project1 that we created, Open your build.gradle(Module) add below lines , these are dependancies we will need in our project

## Glide will be used to Load images, Loopj will help us get data from our API and GSON to make data in the right format to be used by RecyclerAdapter
dependencies {
     
    ..... 
    implementation 'com.github.bumptech.glide:glide:4.4.0'
    implementation 'com.loopj.android:android-async-http:1.4.9'
    implementation "com.google.code.gson:gson:2.8.7"
    .....
}
## Create a New Kotlin Class Named Conference_Room.kt, put below code, this is a model that will define the data fields we will be working on.
class Confrerence_Room {
    var room_id : String = ""
    var room_name : String = ""
    var room_desc : String = ""
    var num_of_persons: String = ""
    var availability : String = ""
    var cost: String = ""
    var image_url: String = ""

}
## Create a new XML layout under res - layout, name this XML single_item.xml, these files defines how our items will be displayed to the user.




<?xml version="1.0" encoding="utf-8"?>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              
    xmlns:tools="http://schemas.android.com/tools"
              
    android:layout_width="match_parent"
    tools:layout_editor_absoluteX="0dp"
    tools:layout_editor_absoluteY="0dp"
    android:layout_height="match_parent"
    xmlns:app="http://schemas.android.com/apk/res-auto">
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:padding="10dp"
        android:orientation="horizontal">
        <androidx.cardview.widget.CardView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            app:cardMaxElevation="12dp"
            app:cardUseCompatPadding="true"
            app:cardPreventCornerOverlap="true"
            android:layout_marginTop="0dp">
            <LinearLayout
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:orientation="vertical">
                <ImageView
                    android:layout_width="300dp"
                    android:layout_height="140dp"
                    android:id="@+id/image_url"
                    android:src="@drawable/img"/>
                <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:orientation="horizontal">
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="Room_Id: "
                        android:layout_marginTop="5dp"
                        android:textSize="20dp"
                        android:textStyle="bold"/>
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="ID"
                        android:textColor="#7B1FA2"
                        android:layout_marginTop="5dp"
                        android:id="@+id/room_id"
                        android:textSize="20dp"
                        android:textStyle="bold"/>

                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:layout_marginTop="5dp"
                        android:textAlignment="center"
                        android:textColor="#3C1AD3"
                        android:layout_marginStart="10dp"
                        android:textStyle="bold"
                        android:text="Name of the room"
                        android:textSize="20dp"
                        android:id="@+id/room_name"/>

                </LinearLayout>

                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="This is my description text"
                    android:layout_marginTop="5dp"
                    android:id="@+id/room_desc"
                    android:textStyle="italic"
                    android:textSize="20dp"/>
                <LinearLayout
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_marginTop="5dp"
                    android:orientation="horizontal">
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="persons"
                        android:id="@+id/num_of_persons"
                        android:textColor="@color/purple_500"
                        android:layout_marginEnd="10dp"
                        android:textStyle="bold"
                        android:textSize="18dp"/>
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="Status: "
                        android:textStyle="bold"
                        android:textSize="18dp"/>
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:layout_marginEnd="10dp"
                        android:id="@+id/availability"
                        android:textColor="#27B32D"
                        android:text="available"
                        android:textStyle="bold"
                        android:textSize="18dp"/>
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="Cost: "
                        android:textStyle="bold"
                        android:textSize="18dp"/>

                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:id="@+id/cost"
                        android:text="Ksh: 20,000"
                        android:textColor="#E64A19"
                        android:textStyle="bold"
                        android:textSize="18dp"/>

                </LinearLayout>
                <Button
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_marginTop="5dp"
                    android:id="@+id/make_reservation"
                    android:text="Make Reservation"
                    android:layout_marginBottom="10dp"/>

            </LinearLayout>
        </androidx.cardview.widget.CardView>

    </LinearLayout>
</LinearLayout>

##  Below we create a Recycler adapter that will connect the XML file and the Model we created on Step 2 and 3, the Recycler adpater also will receive data coming from our API.
package com.example.recycler
import android.content.Context

import android.content.Intent

import android.content.SharedPreferences
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.ImageView
import android.widget.TextView
import androidx.recyclerview.widget.RecyclerView
import com.bumptech.glide.Glide
import com.bumptech.glide.request.RequestOptions


class RecyclerAdapter(var context: Context)://When you want to toast smthg without intent or activities
    RecyclerView.Adapter<RecyclerAdapter.ViewHolder>(){
    //View holder holds the views in single item.xml

    var conferenceRoom : List<Confrerence_Room> = listOf() // empty  list


    inner class ViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {}
    //Note below code returns above class and pass the view
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): RecyclerAdapter.ViewHolder {
        val view = LayoutInflater.from(parent.context).inflate(R.layout.single_item, parent, false)
        return ViewHolder(view)
    }
    //so far item view is same as single item
    override fun onBindViewHolder(holder: RecyclerAdapter.ViewHolder, position: Int) {
        val id = holder.itemView.findViewById(R.id.room_id) as TextView
        val name = holder.itemView.findViewById(R.id.room_name) as TextView
        val desc = holder.itemView.findViewById(R.id.room_desc) as TextView
        val persons = holder.itemView.findViewById(R.id.num_of_persons) as TextView
        val availability = holder.itemView.findViewById(R.id.availability) as TextView
        val cost = holder.itemView.findViewById(R.id.cost) as TextView
        val image = holder.itemView.findViewById(R.id.image_url) as ImageView

        //bind
        val item = conferenceRoom[position]
        id.text=item.room_id
        name.text = item.room_name
        desc.text = item.room_desc
        persons.text=item.num_of_persons
        availability.text=item.availability
        cost.text = item.cost

        Glide.with(context).load(item.image_url)
            .apply(RequestOptions().centerCrop())
            .into(image)

        //image.setImageResource(item.image)

        //        holder.itemView.setOnClickListener {
        //
        //
        //
        //        }
    }

    override fun getItemCount(): Int {
        return conferenceRoom.size
    }

    //we will call this function on Loopj response
    fun setProductListItems(conferenceRoom: List<Confrerence_Room>){
        this.conferenceRoom = conferenceRoom
        notifyDataSetChanged()
    }
}//end class
## Now Go to activity_main.xml and create a recycler View widget and a Progress Bar.
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">
        <ProgressBar
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/progressbar"/>

        <androidx.recyclerview.widget.RecyclerView
            android:layout_width="match_parent"
            android:id="@+id/recycler"
            android:layout_height="match_parent"
            tools:listitem="@layout/single_item"/>
    </LinearLayout>


</androidx.constraintlayout.widget.ConstraintLayout>
## We now head to our MainActivity File and Get data from our API, we will be getting data from below API - (Application Programming Interface), this link will provide us data from a database.
(https://musau.pythonanywhere.com/getconferenceroom)

## Open Your Main Activity and write this Code. Below code gets data from our API and pushes to the recycler adapter which maps the data to our XML.
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.view.View
import android.widget.ProgressBar
import android.widget.Toast
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView
import com.example.recycler.Confrerence_Room
import com.example.recycler.R
import com.example.recycler.RecyclerAdapter
//import com.example.testapi.Product
//import com.example.testapi.RecyclerAdapter
import com.google.gson.GsonBuilder
import com.loopj.android.http.AsyncHttpClient
import com.loopj.android.http.JsonHttpResponseHandler
import cz.msebera.android.httpclient.Header
import cz.msebera.android.httpclient.entity.StringEntity
import org.json.JSONArray
import org.json.JSONObject

class MainActivity : AppCompatActivity() {

    lateinit var confrerenceRoom:ArrayList<Confrerence_Room>
    lateinit var recyclerAdapter: RecyclerAdapter //call the adapter
    lateinit var progressbar: ProgressBar
    lateinit var recyclerView: RecyclerView


    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        recyclerView = findViewById(R.id.recycler)
        progressbar= findViewById(R.id.progressbar)
        progressbar.visibility = View.VISIBLE

        val client = AsyncHttpClient(true,80,443)
        //        //pass the product list to adapter
        recyclerAdapter = RecyclerAdapter(applicationContext)
        recyclerView.layoutManager = LinearLayoutManager(applicationContext)
        recyclerView.setHasFixedSize(true)

        client.get(this, "https://musau.pythonanywhere.com/getconferenceroom",
            null,
            "application/json",
            object: JsonHttpResponseHandler(){
                override fun onSuccess(statusCode: Int, headers: Array<out Header>?, response: JSONArray?) {
                    //we convert json array to a list of a given model
                    val gson = GsonBuilder().create()
                    val list = gson.fromJson(response.toString(),
                        Array<Confrerence_Room>::class.java).toList()
                    //now pass the converted list to adapter
                    recyclerAdapter.setProductListItems(list)
                    progressbar.visibility = View.GONE
                }

                override fun onFailure(
                    statusCode: Int,
                    headers: Array<out Header>?,
                    responseString: String?,
                    throwable: Throwable?
                ) {
                    Toast.makeText(applicationContext, "no products on sale"+statusCode, Toast.LENGTH_LONG).show()
                    progressbar.visibility = View.GONE
                }
            }//end handler
        )//end post

        //now put the adapter to recycler view
        recyclerView.adapter = recyclerAdapter
    }
}
## Incase of errors with compatibilty issues with AndroidX add below file to gradle.properties add below line
android.enableJetifier=true
## You can also add Internet Permissions in AndroidManifest.xml.
  <uses-permission android:name="android.permission.INTERNET"/>
  
  
  You are done, Run Your code on the device
  
## Creatibg a SingleAcitivity page to View a Single Conference room on click
# Right click on app - New - Activity - Empty Activity. Give it a name SingleActivity - Finish
# After Activity is created, head back to your recycler adapter and add below code inside onBindViewHolder function.

 holder.itemView.setOnClickListener {
            //create a shared prefferences variable to store our clicked product
            val prefs: SharedPreferences = context.getSharedPreferences(
                "store",
                Context.MODE_PRIVATE
            )
            //save the product
            val editor: SharedPreferences.Editor = prefs.edit()
            editor.putString("room_id", item.room_id)
            editor.putString("room_name", item.room_name)
            editor.putString("room_desc", item.room_desc)
            editor.putString("num_of_persons", item.num_of_persons)
            editor.putString("availability", item.availability)
            editor.putString("cost", item.cost)
            editor.putString("image_url", item.image_url)
            editor.apply()

            //Navigate to SingleACtivity, Created Earlier
            val i = Intent(context, SingleActivity::class.java)
            i.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK)
            context.startActivity(i)
        }

# NB: above code must be put below Glide code in Your Adapter created in Step 4
## Open the activity_single.xml and put below code, this code shows how a single item wil be displayed.
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal"
    tools:context=".SingleActivity">
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:padding="10dp"
        android:orientation="vertical">
        <androidx.cardview.widget.CardView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            app:cardMaxElevation="12dp"
            app:cardUseCompatPadding="true"
            app:cardPreventCornerOverlap="true"
            android:layout_marginTop="0dp">
            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:orientation="vertical">
                <ImageView
                    android:layout_width="300dp"
                    android:layout_height="140dp"
                    android:id="@+id/image_url"
                    android:src="@drawable/img"/>
                <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:orientation="horizontal">
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="Room_Id: "
                        android:layout_marginTop="5dp"
                        android:textSize="20dp"
                        android:textStyle="bold"/>
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="ID"
                        android:textColor="#7B1FA2"
                        android:layout_marginTop="5dp"
                        android:id="@+id/room_id"
                        android:textSize="20dp"
                        android:textStyle="bold"/>

                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:layout_marginTop="5dp"
                        android:textAlignment="center"
                        android:textColor="#3C1AD3"
                        android:layout_marginStart="10dp"
                        android:textStyle="bold"
                        android:text="Name of the room"
                        android:textSize="20dp"
                        android:id="@+id/room_name"/>

                </LinearLayout>
                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="Description"
                    android:textAlignment="center"
                    android:layout_marginTop="5dp"
                    android:textStyle="bold"
                    android:textSize="30dp"/>

                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="This is my description text"
                    android:layout_marginTop="5dp"
                    android:id="@+id/room_desc"
                    android:textStyle="italic"
                    android:textSize="20dp"/>
                <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:layout_marginTop="5dp"
                    android:orientation="horizontal">
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="Capacity: "
                        android:textColor="@color/purple_500"
                        android:layout_marginEnd="10dp"
                        android:textStyle="bold"
                        android:textSize="18dp"/>
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="persons"
                        android:id="@+id/num_of_persons"
                        android:textColor="@color/purple_500"
                        android:layout_marginEnd="15dp"
                        android:textStyle="bold"
                        android:textSize="18dp"/>
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="Status: "
                        android:textStyle="bold"
                        android:textSize="20dp"/>
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:layout_marginEnd="10dp"
                        android:id="@+id/availability"
                        android:textColor="#27B32D"
                        android:text="available"
                        android:textStyle="bold"
                        android:textSize="18dp"/>

                </LinearLayout>
                <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:orientation="horizontal">
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="Cost: "
                        android:textStyle="bold"
                        android:textSize="22dp"/>

                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:id="@+id/cost"
                        android:text="Ksh: 20,000"
                        android:textColor="#E64A19"
                        android:textStyle="bold"
                        android:textSize="20dp"/>

                </LinearLayout>
                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="Make Reservation Below"
                    android:textColor="#5E309F"
                    android:textStyle="bold"
                    android:layout_marginTop="25dp"
                    android:textSize="30dp"/>
                <EditText
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:hint="Enter your name "
                    android:padding="10dp"
                    android:textSize="20dp"
                    android:id="@+id/name"
                    android:layout_marginBottom="10dp"/>
                <EditText
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:hint="Enter your Email Address"
                    android:padding="10dp"
                    android:textSize="20dp"
                    android:id="@+id/email"
                    android:layout_marginBottom="10dp"/>
                <EditText
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:inputType="date"
                    android:padding="10dp"
                    android:textSize="20dp"
                    android:id="@+id/date"
                    android:layout_marginBottom="10dp"/>
                <EditText
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:padding="10dp"
                    android:inputType="time"
                    android:textSize="20dp"
                    android:id="@+id/start_time"
                    android:layout_marginBottom="10dp"/>
                <EditText
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:padding="10dp"
                    android:inputType="time"
                    android:textSize="20dp"
                    android:id="@+id/end_time"
                    android:layout_marginBottom="10dp"/>
                <EditText
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:hint="Enter room Id"
                    android:padding="10dp"
                    android:textSize="20dp"
                    android:id="@+id/R_id"
                    android:layout_marginBottom="10dp"/>

            </LinearLayout>

        </androidx.cardview.widget.CardView>

    </LinearLayout>


</LinearLayout>

## Open Your Single Activity.kt File and add below codes
package com.example.project1
import android.content.Context
import android.content.SharedPreferences
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.widget.ImageView
import android.widget.TextView
import com.bumptech.glide.Glide
import com.bumptech.glide.request.RequestOptions
class SingleActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_single)

        //access shared prefferences
        val prefs: SharedPreferences = getSharedPreferences("store",
            Context.MODE_PRIVATE)

        //access the saved product_name from prefferences and put in the TextView
        val id = prefs.getString("room_id", "")
        val room_id = findViewById(R.id.room_id) as TextView
        room_id.text = id

        //access the saved product_desc from prefferences and put in the TextView
        val name = prefs.getString("room_name", "")
        val room_name = findViewById(R.id.room_name) as TextView
        room_name.text = name

        //access the saved product_cost from prefferences and put in the TextView
        val desc = prefs.getString("room_desc", "")
        val room_desc= findViewById(R.id.room_desc) as TextView
        room_desc.text = desc

        val persons = prefs.getString("num_of_persons", "")
        val num_persons= findViewById(R.id.num_of_persons) as TextView
        num_persons.text = persons

        val availability = prefs.getString("availability", "")
        val status= findViewById(R.id.availability) as TextView
        status.text = availability

        val cost = prefs.getString("cost", "")
        val room_cost= findViewById(R.id.cost) as TextView
        room_cost.text = cost

        //access the saved image from prefferences and put in the ImageView Using Glide
        val image_url = prefs.getString("image_url", "")
        val image = findViewById(R.id.image_url) as ImageView
        Glide.with(applicationContext).load(image_url)
            .apply(RequestOptions().centerCrop())
            .into(image)
    }
}
# run your code
