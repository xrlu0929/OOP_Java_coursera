package life_expectancy;

import java.util.HashMap;
import java.util.List;

import de.fhpotsdam.unfolding.UnfoldingMap;
import de.fhpotsdam.unfolding.data.Feature;
import de.fhpotsdam.unfolding.data.GeoJSONReader;
import de.fhpotsdam.unfolding.marker.Marker;
import de.fhpotsdam.unfolding.providers.Microsoft;
import de.fhpotsdam.unfolding.utils.MapUtils;
import processing.core.PApplet;

public class LifeExpectancy extends PApplet {
	UnfoldingMap map;
	HashMap<String, Float> LifeExpMap;
	List<Feature> countries;
	List<Marker> countryMarkers;
	
	public void setup() {
		size(800, 600, OPENGL);
		map = new UnfoldingMap(this, 50, 50, 700, 500, new Microsoft.AerialProvider());
		MapUtils.createDefaultEventDispatcher(this, map);
		
		//load data from the form
		LifeExpMap = loadDataFromCSV("LifeExpectancyWorldBankModule3.csv"); //class created below as private
		
		//load country shape
		countries = GeoJSONReader.loadData(this, "countries.geo.json");
		//create markers accordingly
		countryMarkers = MapUtils.createSimpleMarkers(countries);
		//add markers to our map
		map.addMarkers(countryMarkers);
		//shade markers
		shadeCountries();//method created below
	}
	
	private void shadeCountries() {
		for (Marker marker : countryMarkers) 
		{
			// Find data for country of the current marker
			String countryId = marker.getId();
			if (LifeExpMap.containsKey(countryId)) //if countryID is covered in our CSV form
			{
				float lifeExp = LifeExpMap.get(countryId);
				// Encode value(values range: 40-90) as brightness (values range: 10-255)
				int colorLevel = (int) map(lifeExp, 40, 90, 10, 255);
				marker.setColor(color(255-colorLevel, 100, colorLevel));
				//the longer people in the country can life, the "bluer" the country marker is;
				//the shorter people in the country can life, the "redder" the country marker is;
			}
			else //if countryID is not covered in our CSV form
			{
				marker.setColor(color(150,150,150));
			}
		}
	}

	private HashMap<String, Float> loadDataFromCSV(String fileName) {
		HashMap<String, Float> lifeExpMap = new HashMap<String, Float>();

		String[] rows = loadStrings(fileName);
		//look into CSV form and then decide-this part is related to CVS's format
		for (String row : rows) {
			String[] columns = row.split(",");
			if (columns.length == 6 && !columns[5].equals("..")) {
				lifeExpMap.put(columns[4], Float.parseFloat(columns[5]));
			}
		}

		return lifeExpMap;
	}
	
	public void draw() {
		map.draw();
	}

}
