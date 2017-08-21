# foodbuddyfinal
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;


public class MainActivityTrash extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

    }


}

public interface IPlace {

    public String getName();

    public List<String> getCategories();

    public String getGoogleID();

    public String getYelpID();

    public String getPrice();
    // Yelp gives the price in dollar signs. "$", "$$", "$$$", or ""$$$$"

    public String getAddress1();

    public String getAddress2();
    // these next two field will most likely be empty, but MIGHT be used depending on region

    public String getAddress3();

    public String getCity();

    public String getURLYelp();
    // Link to the Yelp page for business

    public String getURLImage();

    public double getRating();
    // gives value such as 1.5, 2, 2.5

    public double getLatitude();

    public double getLongitude();

    public int reviewCount();
    //number of reviews there are
}
