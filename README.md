# MovieQueryApp
import java.awt.*;
import java.awt.event.*;

import javax.swing.*;
import javax.swing.border.Border;
import javax.swing.border.TitledBorder;
import javax.swing.event.DocumentEvent;
import javax.swing.event.DocumentListener;

import java.sql.*;
import java.text.SimpleDateFormat;
import java.time.*;
import java.util.*;


public class hw3 extends JFrame {
	private JFrame frame;
	private Connection connection;
	
	private ArrayList<String> countryList; //country list after selection of genres 
	private ArrayList<String> locationList; //location list after selection of genres and countries
	private ArrayList<String> tagList; //location list after selection of genres and countries	
	private ArrayList<JCheckBox> tagsChecked;
	
	private HashSet<JCheckBox> countries;
	private HashSet<JCheckBox> locations;
	private HashSet<JCheckBox> tags;
	
	public String initialQueryString;
	private String finalResult;
	
	private String queryGenre;
	private String queryCountry;
	private String queryLocation;
	private String queryRating;
	private String queryMovieYear;
	private String queryTag;
	private String queryTagWeight;
	
	public String queryForResult;
	private String queryForCountry;
	private String queryForLocation;
	private String queryForTag;

	
	private JPanel jpGenre;
	private JPanel jpCountry;
	private JPanel jpLocation;
	private JPanel jpSearchBetween;
	private JPanel jpRating;
	private JPanel jpMovieYear;
	private JPanel jpMovieTag;
	private JPanel jpTagWeight;
	private JPanel jpShowQuery;
	private JPanel jpResult;
	
	private JScrollPane scrollPanelGenre;
	private JScrollPane scrollPanelCountry;
	private JScrollPane scrollPanelLocation;
	private JScrollPane scrollPanelTag;
	private JScrollPane scrollPanelShowQuery;
	private JScrollPane scrollPanelResult;
	
	private JComboBox comboBoxSearchBetween;
	
	private JComboBox comboBoxCriticRating;
	private JTextField textCriticRatingValue;
	private JComboBox comboBoxNumReview;
	private JTextField textNumReviewValue;
	
	private JTextField movieFrom;
	private JTextField movieTo;
	
	private JTextArea textMovieTag;
	private JComboBox comboBoxTagWeight;
	private JTextField tagWeightValue;
	
	private JTextArea textShowQuery;
	private JTextArea textResult;
	
	private ArrayList<JCheckBox> genres;
	private JCheckBox genreCheckBox1;
	private JCheckBox genreCheckBox2;
	private JCheckBox genreCheckBox3;
	private JCheckBox genreCheckBox4;
	private JCheckBox genreCheckBox5;
	private JCheckBox genreCheckBox6;
	private JCheckBox genreCheckBox7;
	private JCheckBox genreCheckBox8;
	private JCheckBox genreCheckBox9;
	private JCheckBox genreCheckBox10;
	private JCheckBox genreCheckBox11;
	private JCheckBox genreCheckBox12;
	private JCheckBox genreCheckBox13;
	private JCheckBox genreCheckBox14;
	private JCheckBox genreCheckBox15;
	private JCheckBox genreCheckBox16;
	private JCheckBox genreCheckBox17;
	private JCheckBox genreCheckBox18;
	private JCheckBox genreCheckBox19;
	private JCheckBox genreCheckBox20;
	
	
	public hw3() throws SQLException {
		this.countries = new HashSet<JCheckBox>();
		this.locations = new HashSet<JCheckBox>();
		this.tags = new HashSet<JCheckBox>();
		buildFrame();
		this.connection = dbConnector();
	}
	
	private void buildFrame() throws SQLException {
		this.frame = new JFrame();
		this.frame.setTitle( "Movie" );
		this.frame.setBounds( 100, 100, 1200, 790 );
	    this.frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
	    this.frame.getContentPane().setLayout(null);
	    
	    buildJPGenre();
	    buildJPCountry();
	    buildJPLocation();
	    buildJPRating();
	    buildJPMovieYear();
	    buildJPMovieTag();
	    buildJPTagWeight();
	    buildJPSearchBetween();
	    buildJPShowQuery();
	    buildJPResult();
	    initialQueryString();

	}
	
	private void buildJPSearchBetween() {
		JPanel jpSearchBetween = new JPanel();
		jpSearchBetween.setBorder(new TitledBorder("Search Between Attributes' Values"));
		jpSearchBetween.setLayout(new GridLayout(0, 1, 0, 10));
		JScrollPane scrollPanelSearchBetween= new JScrollPane(jpSearchBetween);
		scrollPanelSearchBetween.setBounds(10, 380,600, 50);      
		this.frame.getContentPane().add(scrollPanelSearchBetween);

		comboBoxSearchBetween = new JComboBox();
		comboBoxSearchBetween.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
	            //
	        }
	     });
		comboBoxSearchBetween.setModel(new DefaultComboBoxModel(new String[] {"Select AND, OR between attribute values", 
				"AND between attribute values", "OR between attribute values"}));
		comboBoxSearchBetween.setBounds(55, 125, 90, 25);
		jpSearchBetween.add(comboBoxSearchBetween);
		
		/*
		comboBoxSearchBetween.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
				setQueryStringSearch();
			}
		});
		*/
	}
	
	private void setQueryString() {
		int index = this.comboBoxSearchBetween.getSelectedIndex();
		//index = 1: AND; index = 2: OR
		
		if ( index >= 1 ){
			this.queryForResult = this.initialQueryString + " AND x1.id IN \n( ";
			
			if ( this.queryGenre != null && this.queryGenre.length() > 0 ){
				this.queryForResult += this.queryGenre;				
			}
			if ( this.queryCountry != null && this.queryCountry.length() > 0 ) {
				this.queryForResult += "\nINTERSECT \n";
				this.queryForResult += this.queryCountry;
			}
			if ( this.queryLocation != null && this.queryLocation.length() > 0 ) {
				this.queryForResult += "INTERSECT \n";
				this.queryForResult += this.queryLocation;
			}
			if ( this.queryRating != null && this.queryRating.length() > 0 ) {
				this.queryForResult += "INTERSECT \n";
				this.queryForResult += this.queryRating;
			}
			if ( this.queryMovieYear != null && this.queryMovieYear.length() > 0 ) {
				this.queryForResult += "INTERSECT \n";
				this.queryForResult += this.queryMovieYear;
			}
			if ( this.queryTag != null && this.queryTag.length() > 0 ) {
				this.queryForResult += "INTERSECT \n";
				this.queryForResult += this.queryTag;
			}
			if ( this.queryTagWeight != null && this.queryTagWeight.length() > 0 ) {
				this.queryForResult += "INTERSECT \n";
				this.queryForResult += this.queryTagWeight;
			} 
			this.queryForResult += ")\n";
			this.queryForResult += "ORDER BY x1.title \n";
		}
		
		System.out.println("Query Sent to DB: \n");
	    System.out.println(this.queryForResult);
	    this.textShowQuery.setText(queryForResult);
	    this.textShowQuery.setAutoscrolls(isDisplayable());
	}
	
	private void buildJPGenre() {
		this.jpGenre = new JPanel();
		this.jpGenre.setBorder(new TitledBorder("Genres")); 
		this.jpGenre.setLayout(new GridLayout(0, 1, 0, 10));
	    this.scrollPanelGenre = new JScrollPane(this.jpGenre);
	    this.scrollPanelGenre.setBounds(10, 30, 200, 350);  
	    this.frame.getContentPane().add(this.scrollPanelGenre);
	    
	    this.genres = new ArrayList<JCheckBox>();
	    this.genreCheckBox1 = new JCheckBox("Action");
	    this.genres.add(this.genreCheckBox1);
	    this.genreCheckBox2 = new JCheckBox("Adventure");
	    this.genres.add(this.genreCheckBox2);
	    this.genreCheckBox3 = new JCheckBox("Animation");
	    this.genres.add(this.genreCheckBox3);
	    this.genreCheckBox4 = new JCheckBox("Children");
	    this.genres.add(this.genreCheckBox4);
	    this.genreCheckBox5 = new JCheckBox("Comedy");
	    this.genres.add(this.genreCheckBox5);
	    this.genreCheckBox6 = new JCheckBox("Crime");
	    this.genres.add(this.genreCheckBox6);
	    this.genreCheckBox7 = new JCheckBox("Documentary");
	    this.genres.add(this.genreCheckBox7);
	    this.genreCheckBox8 = new JCheckBox("Drama");
	    this.genres.add(this.genreCheckBox8);
	    this.genreCheckBox9 = new JCheckBox("Fantasy");
	    this.genres.add(this.genreCheckBox9);
	    this.genreCheckBox10 = new JCheckBox("Film-Noir");
	    this.genres.add(this.genreCheckBox10);
	    this.genreCheckBox11 = new JCheckBox("Horror");
	    this.genres.add(this.genreCheckBox11);
	    this.genreCheckBox12 = new JCheckBox("IMAX");
	    this.genres.add(this.genreCheckBox12);
	    this.genreCheckBox13 = new JCheckBox("Musical");
	    this.genres.add(this.genreCheckBox13);
	    this.genreCheckBox14 = new JCheckBox("Mystery");
	    this.genres.add(this.genreCheckBox14);
	    this.genreCheckBox15 = new JCheckBox("Romance");
	    this.genres.add(this.genreCheckBox15);
	    this.genreCheckBox16 = new JCheckBox("Sci-Fi");
	    this.genres.add(this.genreCheckBox16);
	    this.genreCheckBox17 = new JCheckBox("Short");
	    this.genres.add(this.genreCheckBox17);
	    this.genreCheckBox18 = new JCheckBox("Thriller");
	    this.genres.add(this.genreCheckBox18);
	    this.genreCheckBox19 = new JCheckBox("War");
	    this.genres.add(this.genreCheckBox19);
	    this.genreCheckBox20 = new JCheckBox("Western");
	    this.genres.add(this.genreCheckBox20);
	   
	    for (int i = 0; i < genres.size(); i++) {
	    		jpGenre.add( genres.get(i) );
	    }
	       
	    for (int i = 0; i < genres.size(); i++) {
	    		genres.get(i).addActionListener(new ActionListener() {
	    			public void actionPerformed(ActionEvent arg0) {
	    				try {
	    					setQueryStringGenre();
	    				} catch (SQLException e) {
	    					e.printStackTrace();
	    					}
	    			}
	          });
	    	}
	}
	    	
	private void setQueryStringGenre() throws SQLException {
		ArrayList<JCheckBox> genresChecked = new ArrayList<JCheckBox>();
		
		for(int i = 0; i < genres.size(); i++) {
			if(genres.get(i).isSelected()) {
				genresChecked.add(genres.get(i));
			}
		}
		
		if (genresChecked.size() == 0) { 
			this.queryGenre = "";
			return;
		}

		this.queryGenre = "SELECT distinct mg.movieID from movie_genres mg\n    WHERE ";
		for(int i = 0; i < genresChecked.size(); i++) {
			if ( i != 0 ){
				this.queryGenre += "SELECT distinct mg.movieID from movie_genres mg\n    WHERE ";
			}
			this.queryGenre += "mg.genre = '" +  genresChecked.get(i).getText() + "' ";
			if(i < genresChecked.size() - 1 && this.comboBoxSearchBetween.getSelectedIndex() == 1) {
	            this.queryGenre += "INTERSECT ";
	         }
			else if(i < genresChecked.size() - 1 && this.comboBoxSearchBetween.getSelectedIndex() == 2) {
	            this.queryGenre += "UNION ";
	         }
	      }
	     
	      System.out.println("Query for Genre: \n"+ this.queryGenre + "\n"); 
		
	      //prepare for country query, narrow down country list through results from querying Genres
	      this.queryForCountry = "SELECT DISTINCT mc.country FROM movie_countries mc " + 
	    		  					"WHERE mc.movieID IN (" + this.queryGenre + ")";
	      dbForCountry();
	}
	
	private void dbForCountry() throws SQLException {
		//remove all check boxes in country panel
		if (this.countries != null && this.countries.size() > 0) {
			for (JCheckBox box : this.countries) {
				this.jpCountry.remove(box);
				jpCountry.revalidate();
				jpCountry.repaint();
			}
		}
		
		this.countryList = new ArrayList<String>();
		if (this.connection == null || this.connection.isClosed()) {
			this.connection = dbConnector();
	      }
		

		System.out.println("queryForCountry is: "+queryForCountry);
		Statement statement = connection.createStatement();
		ResultSet rs = statement.executeQuery(this.queryForCountry);
		ResultSetMetaData meta = rs.getMetaData();
		
		while (rs.next()) {
			for (int col = 1; col <= meta.getColumnCount(); col++) {
				countryList.add(rs.getString(col));
			}
		}
		
		Collections.sort(countryList.subList(0, countryList.size()));
		
		for (int i = 0; i < countryList.size(); i++) {
			JCheckBox box = new JCheckBox(countryList.get(i));
			this.countries.add(box);
			jpCountry.add(box);
			jpCountry.revalidate();
			jpCountry.repaint();
		}
		
		for (JCheckBox box : this.countries) {
			box.addActionListener(new ActionListener() {
				public void actionPerformed(ActionEvent arg0) {
					try {
						setQueryStringCountry();
					} catch (SQLException e) {
						e.printStackTrace();
					}
				}
			});
		}
		
		statement.close();
		rs.close();
		closeConnection(this.connection); 
	}
	
	private void buildJPCountry() throws SQLException {
		jpCountry = new JPanel();
		jpCountry.setBorder(new TitledBorder("Country")); 
		jpCountry.setLayout(new GridLayout(0, 1, 0, 10));
		this.scrollPanelCountry = new JScrollPane(jpCountry);
		scrollPanelCountry.setBounds(210, 30, 200, 350);
		this.frame.getContentPane().add(scrollPanelCountry);
		

	}
	
	private void setQueryStringCountry() throws SQLException {
		ArrayList<JCheckBox> countriesChecked = new ArrayList<JCheckBox> ();
		
		for(JCheckBox box : this.countries) {
			if(box.isSelected()) {
				countriesChecked.add(box);
			}
		}
		
	    if (countriesChecked.size() == 0) {
	    		this.queryCountry ="";
	    		return;
	    	}
	    System.out.println("contriesChecked size is : "+countriesChecked.size());
	    this.queryCountry = "  SELECT distinct mc.movieID FROM movie_countries mc  WHERE (";
	      for(int i = 0; i < countriesChecked.size(); i++) {
	         this.queryCountry += "mc.country = '" +  countriesChecked.get(i).getText() + "' ";
	         if(i < countriesChecked.size() - 1) {
	            this.queryCountry += "OR ";
	         }
	      }
	      this.queryCountry += " )\n";
	      System.out.println("Query for Country: \n" + this.queryCountry + "\n"); 
	      
	      //prepare for location query, narrow down country list through results from querying Genres and Country
	      this.queryForLocation = "SELECT DISTINCT mloc.location1 FROM movie_locations mloc " + 
		    		  					"WHERE mloc.movieID IN (" + this.queryGenre + "INTERSECT" + this.queryCountry + ")";
		      
	      dbForLocation();	
	}
	
	private void dbForLocation() throws SQLException {
		if ( this.locations != null && this.locations.size() > 0 ){
			for( JCheckBox box : this.locations ){
				this.jpLocation.remove( box );
				jpLocation.revalidate();
				jpLocation.repaint();
			}
		}
		
		this.locationList = new ArrayList<String>();
		
		if (this.connection == null || this.connection.isClosed()) {
			this.connection = dbConnector();
		}
		
		System.out.println("queryForLocation is: "+queryForLocation);
		Statement statement = connection.createStatement();
		ResultSet rs = statement.executeQuery(this.queryForLocation);
		ResultSetMetaData meta = rs.getMetaData();
		
		while (rs.next()) {
			for (int col = 1; col <= meta.getColumnCount(); col++) {
				if ( rs.getString(col) != null ) {
					locationList.add(rs.getString(col));
					//System.out.println("result: location: "+ rs.getString(col));
				}
			}
		}
		
		Collections.sort(locationList.subList(0, locationList.size()));
		
		for (int i = 0; i < locationList.size(); i++) {
			JCheckBox box = new JCheckBox(locationList.get(i));		
			this.locations.add(box);
			jpLocation.add(box);
			jpLocation.revalidate();
			jpLocation.repaint();
		}
		
		for (JCheckBox box : this.locations) {
			box.addActionListener(new ActionListener() {
				public void actionPerformed(ActionEvent arg0) {
					try {
						setQueryStringLocation();
					} catch (SQLException e) {
						e.printStackTrace();
					}
				}
			});
		}
		
		statement.close();
		rs.close();
		closeConnection(this.connection); 
	}
	
	private void buildJPLocation() {
		jpLocation = new JPanel();
		jpLocation.setBorder(new TitledBorder("Filming Location Country"));
		jpLocation.setLayout(new GridLayout(0, 1, 0, 10));
		this.scrollPanelLocation = new JScrollPane(jpLocation);
		scrollPanelLocation.setBounds(410, 30, 200, 350);
		this.frame.getContentPane().add(scrollPanelLocation);
	}
	
	private void setQueryStringLocation() throws SQLException {
		ArrayList<JCheckBox> locationsChecked = new ArrayList<JCheckBox> ();
		
		for(JCheckBox box : this.locations) {
			if(box.isSelected()) {
				locationsChecked.add(box);
			}
		}
		
	    if (locationsChecked.size() == 0) {
	    		this.queryLocation ="";
	    		return;
	    	}
	    
	    this.queryLocation = "  SELECT DISTINCT mloc.movieID FROM movie_locations mloc  WHERE ";
	      for(int i = 0; i < locationsChecked.size(); i++) {
	    	  	if ( i != 0 ){
	    	  		this.queryLocation += "SELECT DISTINCT mloc.movieID from movie_locations mloc\n    WHERE ";
			}
	    	  	this.queryLocation += "mloc.location1 = '" +  locationsChecked.get(i).getText() + "' ";
	    	  	
	    	  	if(i < locationsChecked.size() - 1 && this.comboBoxSearchBetween.getSelectedIndex() == 1) {
	    	  		this.queryLocation += "INTERSECT ";
	    	  	}
	    	  	else if(i < locationsChecked.size() - 1 && this.comboBoxSearchBetween.getSelectedIndex() == 2) {
	    	  		this.queryLocation += "UNION ";
	    	  	}
	    	  }
	      
	      System.out.println("Query for Location: \n" + this.queryLocation + "\n");  
	}
	
	private void buildJPRating() {
		jpRating = new JPanel();
		jpRating.setBorder(new TitledBorder("Critic's Rating"));
		jpRating.setBounds(615, 30, 250, 250);
		jpRating.setLayout(new GridLayout(0, 1, 0, 10));
		this.frame.getContentPane().add(jpRating);

		JLabel labelRating = new JLabel("Rating:");
		labelRating.setHorizontalAlignment(SwingConstants.LEFT);
		labelRating.setBounds(5, 130, 60, 15);
		jpRating.add(labelRating);
		comboBoxCriticRating = new JComboBox();
		comboBoxCriticRating.setModel(new DefaultComboBoxModel(new String[] {"select =, >, <, >= or <=", "=", ">", "<", ">=", "<="}));
		comboBoxCriticRating.setBounds(55, 125, 90, 25);
		jpRating.add(comboBoxCriticRating);
		comboBoxCriticRating.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
				setQueryStringRating();
			}
		});

		JLabel labelCriticValue = new JLabel("Value:");
		labelCriticValue.setHorizontalAlignment(SwingConstants.LEFT);
		labelCriticValue.setBounds(5, 160, 60, 15);
		jpRating.add(labelCriticValue);

		textCriticRatingValue = new JTextField();
		textCriticRatingValue.setColumns(10);
		textCriticRatingValue.setBounds(55, 267, 90, 25);
		jpRating.add(textCriticRatingValue);
		
		textCriticRatingValue.getDocument().addDocumentListener(new DocumentListener() {
			public void insertUpdate(DocumentEvent e) {
				// TODO Auto-generated method stub
				setQueryStringRating();
			}
			@Override
			public void removeUpdate(DocumentEvent e) {
				// TODO Auto-generated method stub
				setQueryStringRating();
			}
			@Override
			public void changedUpdate(DocumentEvent e) {
				// TODO Auto-generated method stub
				setQueryStringRating();
			}
		});
		
		
		JLabel labelNumReview = new JLabel("Num. of Reviews:");
		labelNumReview.setHorizontalAlignment(SwingConstants.LEFT);
		labelNumReview.setBounds(5, 130, 60, 15);
		jpRating.add(labelNumReview);
		comboBoxNumReview = new JComboBox();
		comboBoxNumReview.setModel(new DefaultComboBoxModel(new String[] {"select =, >, <, >= or <=", "=", ">", "<", ">=", "<="}));
		comboBoxNumReview.setBounds(55, 125, 90, 25);
		jpRating.add(comboBoxNumReview);
		comboBoxNumReview.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
				setQueryStringRating();
			}
		});
		
		
		JLabel labelNumReviewValue= new JLabel("Value:");
		labelNumReviewValue.setHorizontalAlignment(SwingConstants.LEFT);
		labelNumReviewValue.setBounds(5, 160, 60, 15);
		jpRating.add(labelNumReviewValue);

		textNumReviewValue = new JTextField();
		textNumReviewValue.setColumns(10);
		textNumReviewValue.setBounds(55, 267, 90, 25);
		jpRating.add(textNumReviewValue);
		
		textNumReviewValue.getDocument().addDocumentListener(new DocumentListener() {
			public void changedUpdate(DocumentEvent e) {
				setQueryStringRating();
			}
			public void removeUpdate(DocumentEvent e) {
				setQueryStringRating();
			}
			public void insertUpdate(DocumentEvent e) {
				setQueryStringRating();
			}
		});

	}

	
	private void setQueryStringRating() {
		if (this.comboBoxCriticRating.getSelectedIndex() < 1 && this.comboBoxNumReview.getSelectedIndex() < 1) {
			this.queryRating = "";
			return;
		} else {
			this.queryRating = "  SELECT m1.id FROM movies m1 \n      WHERE (";
			
			if (this.comboBoxCriticRating.getSelectedIndex() >= 1) {
				this.queryRating += " m1.rtAllCriticsRating  " + this.comboBoxCriticRating.getSelectedItem().toString() 
									+ " " + this.textCriticRatingValue.getText();
	         }
			
			if (this.comboBoxCriticRating.getSelectedIndex() >= 1 && this.comboBoxNumReview.getSelectedIndex() >= 1) {
				if ( comboBoxSearchBetween.getSelectedIndex() == 1 ) {
					this.queryRating += ")\n  AND ( m1.rtAllCriticsNumReviews ";
				}
				else if ( comboBoxSearchBetween.getSelectedIndex() == 2 ) {
					this.queryRating += ")\n  OR ( m1.rtAllCriticsNumReviews ";
				}
				this.queryRating += this.comboBoxNumReview.getSelectedItem().toString() 
									+ " " + this.textNumReviewValue.getText() + " ";
	         }

	         if (this.comboBoxCriticRating.getSelectedIndex() < 1 && this.comboBoxNumReview.getSelectedIndex() >= 1) {
	            this.queryRating += " m1.rtAllCriticsNumReviews " + this.comboBoxNumReview.getSelectedItem().toString() 
	            						+ " " + this.textNumReviewValue.getText() + " ";
	         }

	         this.queryRating += ")\n";
	         System.out.println("Query for Rating: \n" + this.queryRating);
	      }      
	   }
	
	
	private void buildJPMovieYear() {
		jpMovieYear = new JPanel();
		jpMovieYear.setBorder(new TitledBorder("Movie Year"));
		jpMovieYear.setBounds(615, 280, 250, 150);
		jpMovieYear.setLayout(new GridLayout(0, 1, 0, 10));
		this.frame.getContentPane().add(jpMovieYear);
		
		JLabel labelMovieFrom = new JLabel("From: ");
		labelMovieFrom.setBounds(5, 20, 60, 15);
		jpMovieYear.add(labelMovieFrom);
		movieFrom = new JTextField();
		movieFrom.setBounds(41, 30, 111, 23);
		jpMovieYear.add(movieFrom);
		
		movieFrom.getDocument().addDocumentListener(new DocumentListener() {
			public void changedUpdate(DocumentEvent e) {
				setQueryStringMovieYear();
			}
			
			public void removeUpdate(DocumentEvent e) {
				setQueryStringMovieYear();
			}
			
			public void insertUpdate(DocumentEvent e) {
				setQueryStringMovieYear();
			}
		});
		
		JLabel labelMovieTo = new JLabel("To: ");
		labelMovieTo.setBounds(5, 60, 60, 15);
		jpMovieYear.add(labelMovieTo);
		movieTo = new JTextField();
		movieTo.setBounds(41, 56, 111, 23);
		jpMovieYear.add(movieTo);
		
		movieTo.getDocument().addDocumentListener(new DocumentListener() {
			public void changedUpdate(DocumentEvent e) {
				setQueryStringMovieYear();
			}
			public void removeUpdate(DocumentEvent e) {
	        	 	setQueryStringMovieYear();
			}
	         public void insertUpdate(DocumentEvent e) {
	        	 	setQueryStringMovieYear();
	         }
	    });
	}
	
	private void setQueryStringMovieYear()  {
		if (( !this.movieFrom.getText().isEmpty() && this.movieFrom.getText().length() > 0)
			  || (!this.movieTo.getText().isEmpty() && this.movieTo.getText().length() > 0)) {
				this.queryMovieYear = "  SELECT m2.id FROM movies m2 \n      WHERE (";
		} 
		
		else {
				this.queryMovieYear = "";
				return;
		}
		
		if (!this.movieFrom.getText().isEmpty() && this.movieFrom.getText().length() > 0) {
			this.queryMovieYear += "m2.year >= " + this.movieFrom.getText();
		} 
		
		if ((!this.movieFrom.getText().isEmpty() && this.movieFrom.getText().length() > 0) 
		     && (!this.movieTo.getText().isEmpty() && this.movieTo.getText().length() > 0)) {
			this.queryMovieYear += " AND m2.year <= " + this.movieTo.getText();  
		}

		if ( (this.movieFrom.getText().isEmpty() || this.movieFrom.getText().length() == 0) 
		      && (!this.movieTo.getText().isEmpty() && this.movieTo.getText().length() > 0)) {
		      this.queryMovieYear += " m2.year <= " + this.movieTo.getText();
		 }
		
		this.queryMovieYear += ") \n";
		System.out.println("Query for Movie Year: \n" + this.queryMovieYear + "\n");

		
	}
	

	
	private void dbForTag() throws SQLException {
		//prepare for Movie Tag query, narrow down tag list through results from querying Genres and Country and Filming Location and Rating and Movie Year
	    this.queryForTag = "SELECT DISTINCT t.value FROM movie_tags mt, tags t " 
	    						+ "WHERE t.id = mt.tagID AND mt.movieID IN (" + 
	    						this.queryGenre;
	    if ( this.queryCountry != null && this.queryCountry.length() > 0){
	    		this.queryForTag += "INTERSECT "+ this.queryCountry;
	    }
	    if ( this.queryLocation != null && this.queryLocation.length() > 0){
    			this.queryForTag += "INTERSECT "+ this.queryLocation;
	    }
	    if ( this.queryRating != null && this.queryRating.length() > 0){
    			this.queryForTag += "INTERSECT "+ this.queryRating;
	    }
	    if ( this.queryMovieYear != null && this.queryMovieYear.length() > 0){
    			this.queryForTag += "INTERSECT "+ this.queryMovieYear;
	    }
	    this.queryForTag += ") \n";
	      
	    System.out.println("queryForTag is: " + this.queryForTag );
	    
		if ( this.tags != null && this.tags.size() > 0 ){
			for( JCheckBox box : this.tags ){
				this.jpMovieTag.remove( box );
				jpMovieTag.revalidate();
				jpMovieTag.repaint();
			}
		}
		
		this.tagList = new ArrayList<String>();
		
		if (this.connection == null || this.connection.isClosed()) {
			this.connection = dbConnector();
		}
		
		System.out.println("queryForTag is: "+queryForTag);
		Statement statement = connection.createStatement();
		ResultSet rs = statement.executeQuery(this.queryForTag);
		ResultSetMetaData meta = rs.getMetaData();
		
		while (rs.next()) {
			for (int col = 1; col <= meta.getColumnCount(); col++) {
				if ( rs.getString(col) != null ) {
					tagList.add(rs.getString(col));
					//System.out.println("result: location: "+ rs.getString(col));
				}
			}
		}
		
		Collections.sort(tagList.subList(0, tagList.size()));
		

		
		for (int i = 0; i < tagList.size(); i++) {
			JCheckBox box = new JCheckBox(tagList.get(i));	
			this.tags.add(box);
			this.jpMovieTag.add(box);
			this.jpMovieTag.revalidate();
			this.jpMovieTag.repaint();
		}
		
		for (JCheckBox box : this.tags) {
			box.addActionListener(new ActionListener() {
				public void actionPerformed(ActionEvent arg0) {
					try {
						setQueryStringTag();
					} catch (SQLException e) {
						e.printStackTrace();
					}
				}
			});
		}
		
		statement.close();
		rs.close();
		closeConnection(this.connection); 
	}
	
	private void buildJPMovieTag() {
		this.jpMovieTag = new JPanel();
		this.jpMovieTag.setBorder(new TitledBorder("Movie Tag Values"));
		this.jpMovieTag.setLayout(new GridLayout(0, 1, 0, 10));
		this.scrollPanelTag = new JScrollPane(jpMovieTag);
		scrollPanelTag.setBounds(865, 30, 325,270);
		this.frame.getContentPane().add(scrollPanelTag);
	
	}
	
	private void setQueryStringTag() throws SQLException {
		tagsChecked = new ArrayList<JCheckBox> ();
		
		for(JCheckBox box : this.tags) {
			if(box.isSelected()) {
				tagsChecked.add(box);
			}
		}
		
	    if (tagsChecked.size() == 0) {
	    		this.queryTag ="";
	    		return;
	    	}
	    
	    this.queryTag = "  SELECT DISTINCT mt.movieID FROM movie_tags mt, tags t\n  WHERE ";
	      for(int i = 0; i < tagsChecked.size(); i++) {
	    	  	if ( i != 0 ){
	    	  		this.queryTag += "SELECT DISTINCT mt.movieID from movie_tags mt, tags t\n    WHERE ";
			}
	    	  	this.queryTag += "mt.tagID = t.id AND t.value = '" +  tagsChecked.get(i).getText() + "' ";
	    	  	
	    	  	if(i < tagsChecked.size() - 1 && this.comboBoxSearchBetween.getSelectedIndex() == 1) {
	    	  		this.queryTag += "INTERSECT ";
	    	  	}
	    	  	else if(i < tagsChecked.size() - 1 && this.comboBoxSearchBetween.getSelectedIndex() == 2) {
	    	  		this.queryTag += "UNION ";
	    	  	}
	    	  }
	      
	      System.out.println("Query for Tag Value: \n" + this.queryTag + "\n");  
		
	}
	
	
	private void buildJPTagWeight() {
		this.jpTagWeight = new JPanel();
		this.jpTagWeight.setBorder(new TitledBorder(""));
		this.jpTagWeight.setBounds(865, 300, 325, 130);
		this.jpTagWeight.setLayout(new GridLayout(0, 1, 0, 10));
		this.frame.getContentPane().add(this.jpTagWeight);

	
		
        JButton tagGenButton = new JButton("Generate Tag");
        tagGenButton.setHorizontalAlignment(SwingConstants.LEFT);
        tagGenButton.setBounds(5, 130, 60, 15);

        tagGenButton.addActionListener(new ActionListener() {
        		public void actionPerformed(ActionEvent e) {
        			try {
						dbForTag();
					} catch (SQLException e1) {
						// TODO Auto-generated catch block
						e1.printStackTrace();
					}
        		}
        });
       
        jpTagWeight.add(tagGenButton);
        
        JLabel labelTagWeight = new JLabel("Tag Weight:");
        labelTagWeight.setHorizontalAlignment(SwingConstants.LEFT);
        labelTagWeight.setBounds(5, 150, 60, 15);
        jpTagWeight.add(labelTagWeight);
        
        comboBoxTagWeight = new JComboBox();
        comboBoxTagWeight.setModel(new DefaultComboBoxModel(new String[] {"=, >, <, >= or <=", "=", ">", "<", ">=", "<="}));
        comboBoxTagWeight.setBounds(70, 150, 160, 27);
        jpTagWeight.add(comboBoxTagWeight);
        
        
        comboBoxTagWeight.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
				try {
					setQueryStringTagWeight();
				} catch (SQLException e1) {
					// TODO Auto-generated catch block
					e1.printStackTrace();
				}
			}
		});
        
        
        JLabel labelTWValue = new JLabel("Value:");
        labelTWValue.setHorizontalAlignment(SwingConstants.LEFT);
        labelTWValue.setBounds(16, 358, 82, 16);
        jpTagWeight.add(labelTWValue);
        
        tagWeightValue = new JTextField();
        tagWeightValue.setBounds(122, 358, 160, 27);
        jpTagWeight.add(tagWeightValue);
        tagWeightValue.setColumns(10);

		
		tagWeightValue.getDocument().addDocumentListener(new DocumentListener() {
			public void insertUpdate(DocumentEvent e) {
				// TODO Auto-generated method stub
				try {
					setQueryStringTagWeight();
				} catch (SQLException e1) {
					// TODO Auto-generated catch block
					e1.printStackTrace();
				}
			}
			@Override
			public void removeUpdate(DocumentEvent e) {
				// TODO Auto-generated method stub
				try {
					setQueryStringTagWeight();
				} catch (SQLException e1) {
					// TODO Auto-generated catch block
					e1.printStackTrace();
				}
			}
			@Override
			public void changedUpdate(DocumentEvent e) {
				// TODO Auto-generated method stub
				try {
					setQueryStringTagWeight();
				} catch (SQLException e1) {
					// TODO Auto-generated catch block
					e1.printStackTrace();
				}
			}
		}); 
	}
	
	private void setQueryStringTagWeight() throws SQLException {
		if ( this.comboBoxTagWeight.getSelectedIndex() < 1 ) {
			this.queryTagWeight = "";
			return;
		}
		else  {
			this.queryTagWeight = "SELECT DISTINCT mt.movieID FROM movie_tags mt, tags t WHERE mt.tagWeight "
							+ this.comboBoxTagWeight.getSelectedItem().toString() 
							+ " " + this.tagWeightValue.getText();
			if ( this.comboBoxSearchBetween.getSelectedIndex() == 1 && this.tagsChecked != null && tagsChecked.size() > 0) {
				this.queryTagWeight += " AND mt.tagID = t.id AND (";
				for ( int i = 0; i < tagsChecked.size(); i++ ) {
					this.queryTagWeight += "t.value = '" + tagsChecked.get(i).getText() + "'";
					if ( i!= tagsChecked.size() - 1 ){
						this.queryTagWeight += " OR ";
					}
				}
				this.queryTagWeight += ")";
				 		 
			}
		}
		
		System.out.println( "Query for Tag Weight: \n" + this.queryTagWeight );
	}
	

	
	private void buildJPShowQuery() {
		this.jpShowQuery = new JPanel();
		this.jpShowQuery.setBorder( new TitledBorder("Show Query Here:") );
		this.scrollPanelShowQuery = new JScrollPane( jpShowQuery );
		scrollPanelShowQuery.setBounds( 10, 430, 600, 320 );
		this.frame.getContentPane().add( scrollPanelShowQuery );
		
		this.textShowQuery = new JTextArea();
		textShowQuery.setBounds( 10,480, 600, 270 );
		jpShowQuery.add( textShowQuery );
		
		JButton buttonShowQuery = new JButton( "Get Query" );
		buttonShowQuery.setBounds(10, 450,120,50);
		jpShowQuery.add( buttonShowQuery );
		
		buttonShowQuery.addActionListener( new ActionListener() {
			public void actionPerformed( ActionEvent e ){
				try{
					setQueryString();
				} catch (Exception e2 ){
					e2.printStackTrace();
				}
			}
		});
	    
		
		JButton buttonQuery = new JButton( "Execute Query" );
		buttonQuery.setBounds( 10, 430, 120, 50 );
		jpShowQuery.add( buttonQuery );
		
		buttonQuery.addActionListener( new ActionListener() {
			public void actionPerformed( ActionEvent e ) {
				try {
					generateResult();
				} catch (Exception e2 ){
					e2.printStackTrace();
				}
			}
		});
	}
	

	
	private void generateResult() {
		try { 
			if (this.connection == null || this.connection.isClosed()) {
				this.connection = dbConnector();
	         }
	         this.finalResult = "";
	         ResultSet result = searchAllTuples(this.connection);
	         showMetaDataOfResultSet(result);           
	         showResultSet(result); 
	         if (result == null) {
	            System.out.println("null");
	         }
	         result.close();
	      } catch (SQLException e) { 
	         System.err.println("Errors occurs when communicating with the database server: " + e.getMessage()); 
	      } finally {
	         closeConnection(this.connection);
	      } 

	}

	private void showMetaDataOfResultSet(ResultSet result) throws SQLException { 
		ResultSetMetaData meta = result.getMetaData(); 
		for (int col = 1; col <= meta.getColumnCount(); col++) {
			this.finalResult += "   " + meta.getColumnName(col) + "\t ";
			System.out.print("   " + meta.getColumnName(col) + "\t ");
		}
		
		this.finalResult += "\n";
		System.out.println();;
	} 
	
	private void showResultSet(ResultSet result) throws SQLException {
		ResultSetMetaData meta = result.getMetaData();
		int count = 0;
		while (result.next()) {
			for (int col = 1; col <= meta.getColumnCount(); col++) {
				this.finalResult += "\"" + result.getString(col) + "\"\t";
				//System.out.print("\"" + result.getString(col) + "\"\t"); 
			} 
		    count++;
		    this.finalResult += "\n";
		    System.out.println();
		}
		if (count == 0) {
			this.finalResult += "No Search Result.";
			this.textResult.setText(finalResult);
		} 
		else {
			this.finalResult += "\nCount of Rows: " + count + "\n";
			this.textResult.setText( this.finalResult);
		}
		
		System.out.println(this.finalResult);
		closeConnection(this.connection);
	} 
	

	private ResultSet searchAllTuples(Connection con) throws SQLException {
		Statement stmt = con.createStatement();
		return stmt.executeQuery(this.queryForResult); 
	} 
	
	private void buildJPResult() {
		this.jpResult = new JPanel();
		jpResult.setBorder( new TitledBorder( "Result" ) );
		this.scrollPanelResult = new JScrollPane( jpResult );
		scrollPanelResult.setBounds(620, 430, 575,320 );
		this.frame.getContentPane().add( scrollPanelResult );
		
		textResult = new JTextArea();
		textResult.setColumns(10);
		textResult.setBounds(55,267,90,25);
		jpResult.add( textResult );
	}
	
	public static void main(String[] args) {
		EventQueue.invokeLater( new Runnable() {
			public void run(){
				try{
					hw3 ui = new hw3();
					ui.frame.setVisible( true );
				} catch ( Exception e ) {
					e.printStackTrace();
				}
			}
		} );
	}
	
	private void initialQueryString() {
		this.queryGenre = "";
		this.queryCountry = "";
		this.queryLocation = "";
		this.queryRating = "";
		this.queryMovieYear = "";
		this.queryTag = "";

		this.initialQueryString = "SELECT distinct x1.title as title, \n"
	                  + " x1.year as year, x3.country as country, \n"
	                  + " x4.location1 as location_country, x4.location2 as location_state, "
	                  + " x4.location3 as location_city, x4.location4 as location_street, \n"
	                  + " x1.rtAllCriticsRating as AllCriticsRating, \n"
	                  + " x1.rtTopCriticsRating as TopCriticsRating, \n"
	                  + " x1.rtAudienceRating as AudienceRating, \n"
	                  + " x1.rtAllCriticsNumReviews as AllCriticsNumReviews, \n"
	                  + " x1.rtTopCriticsNumReviews as TopCriticsNumReviews, \n"
	                  + " x1.rtAudienceNumRatings as AudienceNumRatings, \n"
	                  + " c.genre_list as genres \n"
	                  + " FROM movies x1, movie_genres x2, movie_countries x3, movie_locations x4, \n"
	                  + "movie_tags mt, \n"
	                  + "  (select distinct mg.movieID, listagg(mg.genre, ', ') within group\n "
	                  + "             (order by mg.movieID) as genre_list from movie_genres mg\n "
	                  + "             group by mg.movieID) c \n"
	                  + " WHERE x1.id = x2.movieID AND x1.id = x3.movieID AND x1.id = x4.movieID\n"
	                  + " AND x1.id = mt.movieID \n AND x1.id = c.movieID \n";

		System.out.println(this.initialQueryString);
		this.queryForResult = initialQueryString;
	} 
	
	
	public static Connection dbConnector() {
		System.out.println("Connecting to database...");
		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
			Connection conn = DriverManager.getConnection("jdbc:oracle:thin:@192.168.56.10:1521:orcl12c", "system", "oracle");
			return conn;
	      } catch (ClassNotFoundException e) {
	    	  	e.printStackTrace();
	    	  	return null;
	      } catch (SQLException e) {
	    	  	e.printStackTrace();
	    	  	return null;
	      }
	   } 
	
	private void closeConnection(Connection con) {
		try {
			con.close();
			} catch (SQLException e) {
				System.err.println("Cannot close connection: " + e.getMessage());
				}
		}

	
}
