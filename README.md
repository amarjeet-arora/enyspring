 <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-openfeign</artifactId>
    </dependency>





package com.example.UserService;

import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

import com.entity.Hotel;

@FeignClient(name = "HOTEL-SERVICES")
public interface HotelService {
	@GetMapping("/hotels/{hotelID}")
	Hotel getHotel(@PathVariable String hotelID);

}





package com.service;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.UUID;
import java.util.stream.Collectors;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.stereotype.Service;
import org.springframework.web.client.RestTemplate;

import com.entity.Hotel;
import com.entity.Rating;
import com.entity.User;
import com.example.UserService.HotelService;
import com.exception.UserNotFound;
import com.repo.UserRepository;

@Service

public class UserServiceImpl implements UserService {
	@Autowired
	private UserRepository repository;
	@Autowired
	HotelService hotelService;

	@Autowired
	RestTemplate restTemplate;

	@Override
	public User saveUser(User user) {
		String randomId = UUID.randomUUID().toString();
		user.setUserId(randomId);
		return repository.save(user);
	}

	@Override
	public List<User> loadAllUsers() {
		// TODO Auto-generated method stub
		return repository.findAll();
	}

	@Override
	public User getUser(String userId) {
		User user = repository.findById(userId)
				.orElseThrow(() -> new UserNotFound("User with given id not found  " + userId));
		Rating[] ratingData = restTemplate.getForObject("http://RATING-SERVICES/ratings/user/" + user.getUserId(),
				Rating[].class);

		List<Rating> ratings = Arrays.stream(ratingData).toList();

		List<Rating> ratingList = ratings.stream().map(rating -> {
			
			//ResponseEntity<Hotel> forEntity = restTemplate.getForEntity("http://HOTEL-SERVICES/hotels/" + rating.getHotelID(),Hotel.class);
			//Hotel hotel = forEntity.getBody();
			Hotel hotel=hotelService.getHotel(rating.getHotelID());
			//System.out.println("Status Code " + forEntity.getStatusCode());
			rating.setHotel(hotel);
			return rating;
		}).collect(Collectors.toList());
		user.setRatings(ratingList);
		return user;

	}

}
