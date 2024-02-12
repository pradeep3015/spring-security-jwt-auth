package com.example.TokenValidator.Test;

import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.*;

import java.util.Date;
import java.util.HashSet;
import java.util.Set;

import javax.crypto.SecretKey;

import io.jsonwebtoken.security.Keys;

@SpringBootApplication
@RestController
public class TokenManagementAPI {

    // Key for signing the token (change this to a secure key in production)
    private static final SecretKey SECRET_KEY = Keys.secretKeyFor(SignatureAlgorithm.HS256);

    // Token expiration time in milliseconds (e.g., 3 minutes)
    private static final long EXPIRATION_TIME = 3 * 60 * 1000;
 //   private static final long EXPIRATION_TIME = 15 * 1000; // 15 seconds

    // Set to store invalidated tokens
    private static final Set<String> invalidatedTokens = new HashSet<>();

    // API endpoint for generating a JWT token
    @PostMapping("/generateToken")
    public String generateToken(@RequestBody TokenRequest tokenRequest) {
        Date now = new Date();
        Date expiryDate = new Date(now.getTime() + EXPIRATION_TIME);

        return Jwts.builder()
                .setSubject(tokenRequest.getBranchno())
                .claim("teller", tokenRequest.getTeller())
                .setIssuedAt(now)
                .setExpiration(expiryDate)
                .signWith(SECRET_KEY)
                .compact();
    }

 // API endpoint for validating a JWT token
    @PostMapping("/validateToken")
    public boolean validateToken(@RequestBody TokenValidationRequest validationRequest) {
        String token = validationRequest.getToken();
        String branchno = validationRequest.getBranchno();
        String teller = validationRequest.getTeller();
        boolean tellerIsActive = validationRequest.isTellerIsActive();

        System.out.println("Validating token: " + token);
        System.out.println("Branch Number: " + branchno);
        System.out.println("Teller: " + teller);
        System.out.println("Is Teller Active: " + tellerIsActive);

        try {
            Claims claims = Jwts.parser()
                    .setSigningKey(SECRET_KEY)
                    .parseClaimsJws(token)
                    .getBody();

            String branchnoFromToken = claims.getSubject();
            String tellerFromToken = (String) claims.get("teller");

            System.out.println("Branch Number from Token: " + branchnoFromToken);
            System.out.println("Teller from Token: " + tellerFromToken);
System.out.println("invalidatetoken :: "+ invalidatedTokens);
System.out.println("token :: "+ token);
            // Check if the token is invalidated
            if (invalidatedTokens.contains(token)) {
                System.out.println("Token is invalidated");
                return false;
            }

            // Check if the teller is active and token has not expired
            boolean isValid = branchno.equals(branchnoFromToken)
                    && teller.equals(tellerFromToken)
                    && (tellerIsActive || new Date().before(claims.getExpiration()));

            System.out.println("Token validation result: " + isValid);
            return isValid;
        } catch (Exception e) {
            System.out.println("Token validation failed with exception: " + e.getMessage());
            return false;
        }
    }

    // API endpoint for logging out a token
    @PostMapping("/logout")
    public String logoutToken(@RequestHeader("Authorization") String authorizationHeader) {
        String token = authorizationHeader.substring(7); // Remove "Bearer " prefix

        invalidatedTokens.add(token); // Add token to the blacklist
        return "Token logged out successfully";
    }

    // Token request payload class
    static class TokenRequest {
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
    static class TokenValidationRequest {
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

    public static void main(String[] args) {
        SpringApplication.run(TokenManagementAPI.class, args);
    }
}


