# foodbuddyfinal
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;

import com.yelp.fusion.client.models.Business;
import com.yelp.fusion.client.models.Category;
import java.util.ArrayList;
import java.util.List;


import com.yelp.fusion.client.connection.*;
import com.yelp.fusion.client.models.Business;
import com.yelp.fusion.client.models.SearchResponse;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;
import retrofit2.Call;
import retrofit2.Response;
import java.util.Random;


public class MainActivityTrash extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

    }
    
public class MainTest {
    public static void main(String[] args){
        YelpRetriever driver = new YelpRetriever();
        Place place = driver.getPlace("mexican");

        System.out.println("   Name is: " + place.getName());
        System.out.println("   city is: " + place.getCity());

        place = driver.getPlace("mexican");

        System.out.println("   Name is: " + place.getName());
        System.out.println("   city is: " + place.getCity());
        place = driver.getPlace("mexican");
        System.out.println("   Name is: " + place.getName());
        System.out.println("   city is: " + place.getCity());

    }
}

public class YelpRetriever {
    private static final String CLIENT_ID = "eZaHYMz2vnT2t6gz-TgCbg";
    private static final String CLIENT_SECRET = "tUVxNkwQ9lAX6U0kYTHf5UHc8wwf3ihZoJTc1TebgDqhvkp9tcHFhQJPSBQOkyHS";
    YelpFusionApiFactory apiFactory = new YelpFusionApiFactory();
    YelpFusionApi yelpFusionApi = null;
    Map<String, String> params;
    int radius = 10;

    /*
     * Making a new YelpRetriever object with the users location
     */

    // Defaults
    public YelpRetriever(){
        params = new HashMap<>();

        // Tries to authenticate
        try {
            yelpFusionApi = apiFactory.createAPI(CLIENT_ID, CLIENT_SECRET);
        } catch (IOException e) {
            e.printStackTrace();
        }

        //
        params.put("latitude", "34.241474");
        params.put("longitude", "-118.529208");
        params.put("term", "restaurant");
    }

    public YelpRetriever(double latitude, double longitude){
        String latString = new Double(latitude).toString();
        String lonString = new Double(longitude).toString();
        params = new HashMap<>();

        // Tries to authenticate
        try {
            yelpFusionApi = apiFactory.createAPI(CLIENT_ID, CLIENT_SECRET);
        } catch (IOException e) {
            e.printStackTrace();
        }

        //
        params.put("latitude", latString);
        params.put("longitude", lonString);
        params.put("term", "restaurant");
    }

    public void setRadius(int radius){
        this.radius = radius;
    }

    public Place getPlace(String alias){
        Random rand = new Random();
        Business[] businessArray = null;
        Place place = null;

        params.put("categories", alias);

        Call<SearchResponse> call = yelpFusionApi.getBusinessSearch(params);
        try {
            Response<SearchResponse> response = call.execute();
            businessArray = new Business[response.body().getBusinesses().size()];
            response.body().getBusinesses().toArray(businessArray);
        } catch (IOException e) {
            e.printStackTrace();
        }

        int n = rand.nextInt(businessArray.length);
        place = new Place(businessArray[n]);
        return place;
        }
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
class Place implements IPlace {
    Business business = null;
    Category testC = new Category();
    public Place(Business business){
        this.business = business;
    }

    @Override
    public String getName() {
        return business.getName();
    }

    @Override
    public List<String> getCategories() {
        // Gets the titles of all categories and throws them into a list
        List<String> catString = new ArrayList<>();
        for (Category category : business.getCategories()) {
            catString.add(category.getTitle());
        }
        return catString;
    }

    @Override
    public String getGoogleID() {
        return null;
    }

    @Override
    public String getYelpID() {
        return business.getId();
    }

    @Override
    public String getPrice() {
        return business.getPrice();
    }

    @Override
    public String getAddress1() {
        return business.getLocation().getAddress1();
    }

    @Override
    public String getAddress2() {
        return business.getLocation().getAddress2();
    }

    @Override
    public String getAddress3() {
        return business.getLocation().getAddress3();
    }

    @Override
    public String getCity() {
        return business.getLocation().getCity();
    }

    @Override
    public String getURLYelp() {
        return business.getUrl();
    }

    @Override
    public String getURLImage() {
        return business.getImageUrl();
    }

    @Override
    public double getRating() {
        return business.getRating();
    }

    @Override
    public double getLatitude() {
        return business.getCoordinates().getLatitude();
    }

    @Override
    public double getLongitude() {
        return business.getCoordinates().getLongitude();
    }

    @Override
    public int reviewCount() {
        return business.getReviewCount();
    }
}
