package com.example.TokenValidator.Service;
import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Modifying;
import org.springframework.data.jpa.repository.Query;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;


import java.util.Date;
import java.util.HashSet;
import java.util.Set;

import javax.crypto.SecretKey;

import io.jsonwebtoken.security.Keys;
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.Table;
import jakarta.transaction.Transactional;

@SpringBootApplication
@RestController
@Transactional // Add this annotation to make all controller methods transactional

public class TokenManagementAPI {

    // Key for signing the token (change this to a secure key in production)
    private static final SecretKey SECRET_KEY = Keys.secretKeyFor(SignatureAlgorithm.HS256);

    // Token expiration time in milliseconds (e.g., 3 minutes)
    private static final long EXPIRATION_TIME = 60 * 1000; // 30 seconds

    //   private static final long EXPIRATION_TIME = 15 * 1000; // 15 seconds

    // Set to store invalidated tokens
    private static final Set<String> invalidatedTokens = new HashSet<>();

    // Inject the repository for database operations
    private final TokenRepository tokenRepository;

    public TokenManagementAPI(TokenRepository tokenRepository) {
        this.tokenRepository = tokenRepository;
    }

    // API endpoint for generating a JWT token

    @PostMapping("/generateToken")
    public String generateToken(@RequestBody TokenRequest tokenRequest) {
        Date now = new Date();

        // Check if there's an existing token for the given branch number and teller
        TokenEntity existingTokenEntity = tokenRepository.findByBranchnoAndTeller(tokenRequest.getBranchno(), tokenRequest.getTeller());
        if (existingTokenEntity != null && !existingTokenEntity.isExpired()) {
            return existingTokenEntity.getToken(); // Return the existing token if it's not expired
        } else {
            // Calculate the expiration date for the new token
            Date expiryDate = new Date(now.getTime() + EXPIRATION_TIME);

            // Generate a new token
            String token = Jwts.builder()
                    .setSubject(tokenRequest.getBranchno())
                    .claim("teller", tokenRequest.getTeller())
                    .setIssuedAt(now)
                    .setExpiration(expiryDate)
                    .signWith(SECRET_KEY)
                    .compact();

            // Save the new token in the database or update the existing token's expiry date
            if (existingTokenEntity != null) {
            	   existingTokenEntity.setToken(token); // Update token
            	    existingTokenEntity.setExpiryDate(expiryDate); // Update expiry date
            	    tokenRepository.save(existingTokenEntity);
            } else {
                TokenEntity tokenEntity = new TokenEntity(token, tokenRequest.getBranchno(), tokenRequest.getTeller(), expiryDate);
                tokenRepository.save(tokenEntity);
            }

            return token;
        }
    }


    // API endpoint for validating a JWT token
//    @PostMapping("/validateToken")
//    public ResponseEntity<String> validateToken(@RequestBody TokenValidationRequest validationRequest) {
//        String token = validationRequest.getToken();
//        String branchno = validationRequest.getBranchno();
//        String teller = validationRequest.getTeller();
//        boolean tellerIsActive = validationRequest.isTellerIsActive();
//
//        try {
//            // Retrieve token from the database
//            TokenEntity tokenEntity = tokenRepository.findByToken(token);
//
//            // Check if token exists and is not expired
//            if (tokenEntity != null && !tokenEntity.isExpired()) {
//                String branchnoFromToken = tokenEntity.getBranchno();
//                String tellerFromToken = tokenEntity.getTeller();
//
//                // Check if the token is invalidated
//                if (invalidatedTokens.contains(token)) {
//                    return ResponseEntity.status(HttpStatus.UNAUTHORIZED).body("Token is invalidated");
//                }
//
//                // Check if the teller is active
//                boolean isValid = branchno.equals(branchnoFromToken)
//                        && teller.equals(tellerFromToken)
//                        && (tellerIsActive || new Date().before(tokenEntity.getExpiryDate()));
//
//                if (isValid) {
//                    return ResponseEntity.ok("Token validation successful");
//                } else {
//                    return ResponseEntity.status(HttpStatus.UNAUTHORIZED).body("Token validation failed");
//                }
//            } else {
//                // Token not found or expired
//                return ResponseEntity.status(HttpStatus.UNAUTHORIZED).body("Token not found or expired");
//            }
//        } catch (Exception e) {
//            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body("Token validation failed with exception: " + e.getMessage());
//        }
//    }
    
    
    @PostMapping("/validateToken")
    public ResponseEntity<String> validateToken(@RequestBody TokenValidationRequest validationRequest) {
        String token = validationRequest.getToken();
        String branchno = validationRequest.getBranchno();
        String teller = validationRequest.getTeller();
        boolean tellerIsActive = validationRequest.isTellerIsActive();

        try {
            // Retrieve token from the database
            TokenEntity tokenEntity = tokenRepository.findByToken(token);

            // Check if token exists and is not expired
            if (tokenEntity != null && !tokenEntity.isExpired()) {
                String branchnoFromToken = tokenEntity.getBranchno();
                String tellerFromToken = tokenEntity.getTeller();

                // Check if the token is invalidated
                if (invalidatedTokens.contains(token)) {
                    return ResponseEntity.status(HttpStatus.UNAUTHORIZED).body("Token is invalidated");
                }

                // Check if the teller is active
                boolean isValid = branchno.equals(branchnoFromToken)
                        && teller.equals(tellerFromToken)
                        && (tellerIsActive || new Date().before(tokenEntity.getExpiryDate()));

                if (isValid) {
                    // Extend token expiry time
                    Date newExpiryDate = new Date(System.currentTimeMillis() + EXPIRATION_TIME);
                    tokenEntity.setExpiryDate(newExpiryDate);
                    tokenRepository.save(tokenEntity);
                    
                    return ResponseEntity.ok("Token validation successful. Expiry date updated to: " + newExpiryDate);
                } else {
                    return ResponseEntity.status(HttpStatus.UNAUTHORIZED).body("Token validation failed");
                }
            } else {
                // Token not found or expired
                return ResponseEntity.status(HttpStatus.UNAUTHORIZED).body("Token not found or expired");
            }
        } catch (Exception e) {
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body("Token validation failed with exception: " + e.getMessage());
        }
    }


    // API endpoint for logging out a token
    @PostMapping("/logout")
    public ResponseEntity<String> logoutToken(@RequestHeader("Authorization") String authorizationHeader) {
        String token = authorizationHeader.substring(7); // Remove "Bearer " prefix

        // Add token to the blacklist
        invalidatedTokens.add(token);

        // Remove token from the database
        tokenRepository.deleteByToken(token);

        return ResponseEntity.ok("Token logged out successfully");
    }

    public static void main(String[] args) {
        SpringApplication.run(TokenManagementAPI.class, args);
    }

}

// Token request payload class
class TokenRequest {
    private String branchno;
    private String teller;

    public String getBranchno() {
        return branchno;
    }

    public void setBranchno(String branchno) {
        this.branchno = branchno;
    }

    public String getTeller() {
        return teller;
    }

    public void setTeller(String teller) {
        this.teller = teller;
    }
}

// Token validation request payload class
class TokenValidationRequest {
    private String token;
    private String branchno;
    private String teller;
    private boolean tellerIsActive;

    public String getToken() {
        return token;
    }

    public void setToken(String token) {
        this.token = token;
    }

    public String getBranchno() {
        return branchno;
    }

    public void setBranchno(String branchno) {
        this.branchno = branchno;
    }

    public String getTeller() {
        return teller;
    }

    public void setTeller(String teller) {
        this.teller = teller;
    }

    public boolean isTellerIsActive() {
        return tellerIsActive;
    }

    public void setTellerIsActive(boolean tellerIsActive) {
        this.tellerIsActive = tellerIsActive;
    }
}

// Entity class to represent a token in the database
@Entity
class TokenEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String token;
    private String branchno;
    private String teller;
    private Date expiryDate;

    public TokenEntity() {
    }

    public TokenEntity(String token, String branchno, String teller, Date expiryDate) {
        this.token = token;
        this.branchno = branchno;
        this.teller = teller;
        this.expiryDate = expiryDate;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getToken() {
        return token;
    }

    public void setToken(String token) {
        this.token = token;
    }

    public String getBranchno() {
        return branchno;
    }

    public void setBranchno(String branchno) {
        this.branchno = branchno;
    }

    public String getTeller() {
        return teller;
    }

    public void setTeller(String teller) {
        this.teller = teller;
    }

    public Date getExpiryDate() {
        return expiryDate;
    }

    public void setExpiryDate(Date expiryDate) {
        this.expiryDate = expiryDate;
    }

    public boolean isExpired() {
        return new Date().after(expiryDate);
    }
}

// Repository interface for database operations
interface TokenRepository extends JpaRepository<TokenEntity, Long> {
    TokenEntity findByToken(String token);
    TokenEntity findByBranchnoAndTeller(String branchno, String teller);

    void deleteByBranchnoAndTeller(String branchno, String teller);

    @Transactional
    @Modifying
    @Query("DELETE FROM TokenEntity t WHERE t.expiryDate <= CURRENT_TIMESTAMP")
    void deleteExpiredTokens();
    void deleteByToken(String token);
}
