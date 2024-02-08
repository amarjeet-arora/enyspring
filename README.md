package com.service;

import java.util.ArrayList;
import java.util.List;
import java.util.UUID;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.web.client.RestTemplate;

import com.entity.Rating;
import com.entity.User;
import com.exception.UserNotFound;
 import com.repo.UserRepository;
@Service

public class UserServiceImpl implements UserService{
	@Autowired
	private UserRepository repository;
	
	@Autowired
	  RestTemplate restTemplate;

	@Override
	public User saveUser(User user) {
		String randomId= UUID.randomUUID().toString();
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
		 User user= repository.findById(userId).
orElseThrow(() -> new UserNotFound("User with given id not found  " + userId));
		 ArrayList<Rating> ratingData= restTemplate.getForObject("http://localhost:8083/ratings/user/"+user.getUserId(),ArrayList.class);
		 user.setRatings(ratingData);
		 System.out.println(ratingData);
		 return user;
	}

}
