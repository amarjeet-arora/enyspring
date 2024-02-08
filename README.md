spring.data.mongodb.uri=mongodb+srv://upgrad:upgrad123@cluster0.rle5i.mongodb.net/?retryWrites=true&w=majority
spring.data.mongodb.database=enydb

package com.entity;

import org.springframework.data.annotation.Id;
import org.springframework.data.mongodb.core.mapping.Document;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
@Document
public class Rating {
	@Id
	private String ratingID;
	private String userID;
	private String hotelID;
	private int rating;
	private String feedback;

}


package com.repo;

import org.springframework.data.mongodb.repository.MongoRepository;
import org.springframework.stereotype.Repository;

import com.entity.Rating;
@Repository
public interface RatingRepo extends MongoRepository<Rating, String>{
	

}




package com.service;

import java.util.List;

import com.entity.Rating;

public interface RatingService {
	
	public Rating create(Rating rating);
	List<Rating> getRatings();
	List<Rating> getRatingByUserID(String userID);
	List<Rating> getRatingByHotelID(String hotelID);
}

