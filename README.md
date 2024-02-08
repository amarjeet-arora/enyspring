package com.repo;

import java.util.List;

import org.springframework.data.mongodb.repository.MongoRepository;
import org.springframework.stereotype.Repository;

import com.entity.Rating;
@Repository
public interface RatingRepo extends MongoRepository<Rating, String>{
	
	List<Rating> findByUserID(String userID);
	List<Rating> findByHotelID(String hotelID);

}





package com.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.entity.Rating;
import com.repo.RatingRepo;

@Service
public class RatingServiceImpl implements RatingService {
	@Autowired
	private RatingRepo repo;

	@Override
	public Rating create(Rating rating) {

		return repo.save(rating);
	}

	@Override
	public List<Rating> getRatings() {

		return repo.findAll();
	}

	@Override
	public List<Rating> getRatingByUserID(String userID) {

		return repo.findByUserID(userID);
	}

	@Override
	public List<Rating> getRatingByHotelID(String hotelID) {

		return repo.findByHotelID(hotelID);
	}

}
